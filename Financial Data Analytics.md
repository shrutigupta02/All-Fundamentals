## Resume Breakdown:
#### prod ready:
- demonstrate the use of ENV file
- "I externalized all secrets and credentials from the source code into environment variables, which is a best practice for production security."
#### secure session mngmt:
- **API session management** is made secure with the use of .env file and dotenv library
#### error handling:
- implemented try and catch blocks throughout the application, that log error and also checks application state, so if any of the fetch requests fail, the application detects it and returns a 503 service unavailable error, preventing any stale or incomplete data.
#### 10,000+ historical stock data points:
- analysed financial data at an interval of 1min, which gives about 9700 data points per month
- 
#### automated calculation algorithms:
calculates SMA, average close values for option trading in selected stocks
#### sub-second response time:
#### real-time trading analytics:

#### scalable data processing pipeline:
#### type validation and error recovery:
- type validation: as API data can come in as strings, and to perform calculations on it i parsed points like "close price", "open price" etc as Float variable using parseFloat()
- error recovery: the api call tries for a total of 3 times, which is if the call fails the first time, it will be called again for two more times before logging an error.
	- **"To make the service resilient to transient network issues, I implemented an error recovery strategy with a retry mechanism. If an API call fails, it will automatically retry up to three times before logging a hard failure.**"
