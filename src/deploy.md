# Deployment of Fingerprint BotD
BotD is an open source library that detects basic bots in a web browser. It detects the presence of automation tools and frameworks. Deployment of this BotD does not require any server as it runs 100% on the client.

<pre><code>
// Initialize the agent at application startup.
const botdPromise = import('https://openfpcdn.io/botd/v1').then((Botd) => Botd.load())
// Get detection results when you need them.
botdPromise
    .then((botd) => botd.detect())
    .then((result) => console.log(result))
    .catch((error) => console.error(error))
</code></pre>

This JavaScript snippet is designed to integrate and utilize the BotD (Bot Detection) library provided by FingerPrint. 

The script is responsible for asynchronously loading the BotD library, initializing the bot detection agent, and retrieving detection results, which can be used to identify and analyze bot activity on a web application.

Promises are used extensively to handle the asynchronous nature of module loading and bot detection. This ensures that each step (loading the module, initializing the agent, performing detection) completes before moving to the next step.

In the final workflow, a user visits the phishing link and must pass the Turnstile authentication to verify they are human. Next, FingerprintJS BotD checks are conducted to prevent bot traffic. Upon successful verification, the user is redirected to the phishing site where their ID and password are captured and stored in the database.
