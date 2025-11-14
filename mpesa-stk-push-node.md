What is STK Push?

STK Push leverages the SIM Application Toolkit to send a prompt directly to a customer’s phone, asking them to enter their M-Pesa PIN to complete a payment. It eliminates the need for customers to remember paybill numbers, account numbers, or transaction codes, significantly reducing friction in the payment process.


### Prerequisites

- A Safaricom Developer Account (register at developer.safaricom.co.ke)
- M-Pesa API credentials (Consumer Key and Secret)
- Node.js and npm installed
- Basic knowledge of Express.js
- A publicly accessible URL for callbacks (we’ll use ngrok for development)

### API credentials:

- CONSUMER_KEY=your_consumer_key_here
- CONSUMER_SECRET=your_consumer_secret_here
- BUSINESS_SHORT_CODE=174379  # Default sandbox shortcode
- PASS_KEY=bfb279f9aa9bdbcf158e97dd71a467cd2e0c893059b10f78e6b72ada1ed2c919  # Default sandbox passkey
- CALLBACK_URL=https://your-callback-url.com/api/mpesa/callback


### The payload definition

- **BusinessShortCode:** The business shortcode.
- **Password:** The generated password.
- **Timestamp:** The current timestamp.
- **TransactionType:** The type of transaction (CustomerPayBillOnline).
- **Amount:** The amount to be charged.
- **PartyA:** The customer's phone number.
- **PartyB:** The business shortcode.
- **PhoneNumber:** The customer's phone number again.
- **CallBackURL:** A URL where M-Pesa will send transaction status updates.
- **AccountReference:** A reference for the account.
- **TransactionDesc:** A description of the transaction.

###  1. TimeStamp format

TimeStamp ensures that each transaction request is time-specific and unique. This prevents resending a valid request to perform unauthorized transactions.

We will need a utility function that converts Date to a string formatted as, YYYYMMDDHHMMSS.

The timestamp is included in the payload when initiating the stk push and represents the exact time when the request is generated.

```js
//timeStamp.ts

const date = new Date();
export const timestamp =
  date.getFullYear() +
  ("0" + (date.getMonth() + 1)).slice(-2) +
  ("0" + date.getDate()).slice(-2) +
  ("0" + date.getHours()).slice(-2) +
  ("0" + date.getMinutes()).slice(-2) +
  ("0" + date.getSeconds()).slice(-2);
```

```js
const getTimestamp = () => {
    const date = new Date();
    const year = date.getFullYear();
    const month = String(date.getMonth() + 1).padStart(2, '0');
    const day = String(date.getDate()).padStart(2, '0');
    const hours = String(date.getHours()).padStart(2, '0');
    const minutes = String(date.getMinutes()).padStart(2, '0');
    const seconds = String(date.getSeconds()).padStart(2, '0');

    return `${year}${month}${day}${hours}${minutes}${seconds}`;
};
```

### 2. Password Generation

The password for STK Push is a Base64 encoded string combining your business shortcode, a passkey provided by Safaricom, and the timestamp:

```js
const getPassword = (timestamp) => {
    const shortCode = process.env.BUSINESS_SHORT_CODE;
    const passKey = process.env.PASS_KEY;
    const password = `${shortCode}${passKey}${timestamp}`;
    return Buffer.from(password).toString('base64');
};
```

### 3. Access Token 

Before making any API calls, you need to authenticate with Safaricom using OAuth:

```js
const getAccessToken = async () => {
    try {
        const url = 'https://sandbox.safaricom.co.ke/oauth/v1/generate?grant_type=client_credentials';
        const auth = Buffer.from(`${process.env.CONSUMER_KEY}:${process.env.CONSUMER_SECRET}`).toString('base64');

        const response = await axios.get(url, {
            headers: {
                'Authorization': `Basic ${auth}`,
            }
        });
        return response.data.access_token;
    } catch(error) {
        console.error('Error getting access token:', error);
        throw error;
    }
};
```

The access token is valid for about an hour and should be used for all subsequent API calls.

#### Complete API

```js
const express = require('express');
const axios = require('axios');
const dotenv = require('dotenv');
const router = express.Router();

dotenv.config();

// Helper function to get the timestamp in YYYYMMDDHHmmss
const getTimestamp = () => {
    const date = new Date();
    const year = date.getFullYear();
    const month = String(date.getMonth() + 1).padStart(2, '0');
    const day = String(date.getDate()).padStart(2, '0');
    const hours = String(date.getHours()).padStart(2, '0');
    const minutes = String(date.getMinutes()).padStart(2, '0');
    const seconds = String(date.getSeconds()).padStart(2, '0');

    return `${year}${month}${day}${hours}${minutes}${seconds}`;
};

// Helper function to generate password
const getPassword = (timestamp) => {
    const shortCode = process.env.BUSINESS_SHORT_CODE;
    const passKey = process.env.PASS_KEY;
    const password = `${shortCode}${passKey}${timestamp}`;
    return Buffer.from(password).toString('base64');
};

// Function to get OAuth token
const getAccessToken = async () => {
    try {
        const url = 'https://sandbox.safaricom.co.ke/oauth/v1/generate?grant_type=client_credentials';
        const auth = Buffer.from(`${process.env.CONSUMER_KEY}:${process.env.CONSUMER_SECRET}`).toString('base64');

        const response = await axios.get(url, {
            headers: {
                'Authorization': `Basic ${auth}`,
            }
        });
        return response.data.access_token;
    } catch(error) {
        console.error('Error getting access token:', error);
        throw error;
    }
};

// Route to initiate STK Push
router.post('/stk-push', async (req, res) => {
  try {
    const { phoneNumber, amount } = req.body;
    
    // Validate inputs
    if (!phoneNumber || !amount) {
      return res.status(400).json({ error: 'Phone number and amount are required' });
    }
    
    // Format phone number
    let formattedPhone = phoneNumber;
    if (phoneNumber.startsWith('0')) {
      formattedPhone = `254${phoneNumber.slice(1)}`;
    } else if (phoneNumber.startsWith('+254')) {
      formattedPhone = phoneNumber.slice(1);
    }
    
    // Get access token
    const accessToken = await getAccessToken();
    
    // Prepare STK Push request
    const timestamp = getTimestamp();
    const password = getPassword(timestamp);
    const shortCode = process.env.BUSINESS_SHORT_CODE;
    
    const url = 'https://sandbox.safaricom.co.ke/mpesa/stkpush/v1/processrequest';
    const data = {
      BusinessShortCode: shortCode,
      Password: password,
      Timestamp: timestamp,
      TransactionType: 'CustomerPayBillOnline',
      Amount: amount,
      PartyA: formattedPhone,
      PartyB: shortCode,
      PhoneNumber: formattedPhone,
      CallBackURL: process.env.CALLBACK_URL,
      AccountReference: 'Test Payment',
      TransactionDesc: 'Test Payment'
    };
    
    // Make STK Push request
    const response = await axios.post(url, data, {
      headers: {
        'Authorization': `Bearer ${accessToken}`,
        'Content-Type': 'application/json'
      }
    });
    
    // Return response to client
    return res.json({
      success: true,
      message: 'STK Push initiated successfully',
      data: response.data
    });
  } catch (error) {
    console.error('Error initiating STK Push:', error.response?.data || error.message);
    return res.status(500).json({
      success: false,
      message: 'Failed to initiate STK Push',
      error: error.response?.data || error.message
    });
  }
});

// Callback route to receive STK Push response
router.post('/callback', (req, res) => {
  console.log('STK Callback response:', JSON.stringify(req.body));
  
  // Extract info from callback
  const callbackData = req.body.Body.stkCallback;
  
  // Always respond to Safaricom with a success to acknowledge receipt
  res.json({ ResultCode: 0, ResultDesc: 'Accepted' });
  
  // Process the callback data as needed for your application
  if (callbackData.ResultCode === 0) {
    // Payment successful
    const transactionDetails = callbackData.CallbackMetadata.Item;
    // Process the successful payment
    console.log('Payment successful');
  } else {
    // Payment failed
    console.log('Payment failed:', callbackData.ResultDesc);
  }
});

module.exports = router;

// In your app.js or index.js file:
const app = express();
app.use(express.json());
app.use('/api/mpesa', router);

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```