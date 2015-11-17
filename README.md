[![NPM version][npm-version-image]][npm-url] [![NPM downloads][npm-downloads-image]][npm-url] [![MIT License][license-image]][license-url] [![Build Status][travis-image]][travis-url]

# TeleSign SDK for NodeJS

A NodeJS wrapper for the TeleSign REST APIs.

Install via npm:

    npm install --save telesign

## SDK Setup

Require the module and call ``setup()``:

    var telesign = require('telesign').setup({
      customerId: 'AAAAAAAA-BBBB-CCCC-DDDD-EEEEEEEEEEEE',
      apiKey: 'your/APIKey=='
    });

You can find your Customer ID and API Key on the TelePortal at [https://teleportal.telesign.com/](https://teleportal.telesign.com/) (leave your API Key in the base64 format as listed).

### Optional Setup Parameters

- ``version`` - API version as a numeric string ***defaults to '1'***
- ``authMethod`` - either ``sha1`` or ``sha256`` ***defaults to 'sha1'***
- ``timeout`` - request timeout, in milliseconds, before returning an error ***defaults to 3000***

## Available Endpoints

For resources that require a ``phoneNumber``, provide a "complete phone number composed of a string of digits without spaces or punctuation, beginning with the Country Dialing Code (e.g., “1” for North America)."

For resources that require a ``ucid``, see the list of available Use Case Codes at [http://docs.telesign.com/rest/content/xt/xt-use-case-codes.html#xref-use-case-codes](http://docs.telesign.com/rest/content/xt/xt-use-case-codes.html#xref-use-case-codes).

## PhoneID

### Get PhoneID Standard

The PhoneID Standard web service returns information about a specified phone number.

    var options = {
      phoneNumber: '15551234567', // required
    };

    telesign.phoneId.score(options, function(err, response) {
      // err: failed request or error in TeleSign response
      // response: JSON response from TeleSign
    });

### Get PhoneID Score

The PhoneID Score web service provides risk information about a specified phone number.

    var options = {
      phoneNumber: '15551234567', // required
      ucid: 'BACF' // required
    };

    telesign.phoneId.score(options, function(err, response) {
      // err: failed request or error in TeleSign response
      // response: JSON response from TeleSign
    });

### Get PhoneID Contact

The PhoneID Contact web service provides contact details for a specified phone number’s subscriber

    var options = {
      phoneNumber: '15551234567', // required
      ucid: 'BACF' // required
    };

    telesign.phoneId.contact(options, function(err, response) {
      // err: failed request or error in TeleSign response
      // response: JSON response from TeleSign
    });

### Get PhoneID Live

The PhoneID Live web service provides information about a specified phone number’s state of operation

**Note: The information returned by PhoneID Live includes the information returned by PhoneID Standard.**

    var options = {
      phoneNumber: '15551234567', // required
      ucid: 'BACF' // required
    };

    telesign.phoneId.live(options, function(err, response) {
      // err: failed request or error in TeleSign response
      // response: JSON response from TeleSign
    });

## Verify

### Verify by Call

The TeleSign Verify Call web service sends a verification code to your end user in a voice message with a phone call.

    var options = {
      phoneNumber: '15551234567', // required
      ucid: 'BACF', // optional
      originatingIp: '', // optional
      language: '', // optional, defaults to 'en-US'
      verifyCode: 12345, // optional, defaults to random value generated by TeleSign
      extensionType: 1, // optional, 1 for automated attendants, 2 for live operators, not included by default
      extensionTemplate: '1,,333' // optional, required if extensionType is specified, not included by default,
      redial: true // optional, defaults to true
    };

    telesign.verify.call(options, function(err, response) {
      // err: failed request or error in TeleSign response
      // response: JSON response from TeleSign
    });

### Verify by SMS

The TeleSign Verify SMS web service sends a verification code to your end user in a text message.

    var options = {
      phoneNumber: '15551234567', // required
      ucid: 'BACF', // optional
      originatingIp: '', // optional
      language: '', // optional, defaults to 'en-US'
      verifyCode: 12345, // optional, defaults to random value generated by TeleSign
      template: 'Your code is $$CODE$$' // optional, must include a $$CODE$$ placeholder to integrate the verifyCode token
    };

    telesign.verify.sms(options, function(err, response) {
      // err: failed request or error in TeleSign response
      // response: JSON response from TeleSign
    });

### Verify by 2-Way SMS

The TeleSign Verify 2-Way SMS web service allows you to authenticate your users and verify user transactions via two-way Short Message Service (SMS) wireless communication. Verification requests are sent to users in a text message, and users return their verification responses by replying to the text message.

    var options = {
      phoneNumber: '15551234567', // required
      ucid: 'BACF', // required
      message: 'Test message with code "$$CODE$$" (reply "YES $$CODE$$" or "NO $$CODE$$")', // required, must include a $$CODE$$ placeholder to integrate the verifyCode token
      validityPeriod: '2m' // required, a string consisting of a number followed by the period of time ('s' for seconds, 'm' for minutes', 'h' for hours, 'd' for days)
    };

    telesign.verify.twoWaySms(options, function(err, response) {
      // err: failed request or error in TeleSign response
      // response: JSON response from TeleSign
    });

### Verify by Push

The TeleSign Verify Push web service is a server-side component of the TeleSign AuthID application, and it allows you to provide on-device transaction authorization for your users. It works by delivering authorization requests to your users via Push Notification, and then by receiving their permission responses via their mobile device’s wireless Internet connection. The service provides two levels of security to support two types of transactions.

    var options = {
      phoneNumber: '15551234567', // required
      notificationType: 'SIMPLE', // optional, either 'CODE' or 'SIMPLE', defaults to 'SIMPLE'
      notificationValue: 719650, // optional, defaults to random value generated by TeleSign
      template: 'mobile_2fa', // required, TeleSign Client Services provides the template name when you enable this from your account and provide us with your artwork,
      message: 'Enter the code display on our web site.' // optional, automatic if not provided
    };

    telesign.verify.push(options, function(err, response) {
      // err: failed request or error in TeleSign response
      // response: JSON response from TeleSign
    });

### Verify Soft Token

The TeleSign Verify Soft Token web service is a server-side component of the TeleSign AuthID application, and it allows you to authenticate your end users when they use the TeleSign AuthID application on their mobile device to generate a Time-based One-time Password (TOTP) verification code.

    var options = {
      phoneNumber: '15551234567', // required
      softTokenId: '', // optional
      verifyCode: 719650, // required, the verification code received from the end user
    };

    telesign.verify.push(options, function(err, response) {
      // err: failed request or error in TeleSign response
      // response: JSON response from TeleSign
    });

### Verify Registration

The TeleSign Verify Registration web service allows you to query TeleSign to determine the current state of the AuthID application registration.

    var options = {
      phoneNumber: '15551234567', // required
      mobileAppSignature: '' // optional
    };

    telesign.verify.verifyRegistration(options, function(err, response) {
      // err: failed request or error in TeleSign response
      // response: JSON response from TeleSign
    });

### Get the Verification Results

Once a verificaiton has been placed via call or SMS, you can get the results of the verification at any time using the reference ID that is sent back after your verification request is placed.

    telesign.verify.get({
      referenceId: '11111BB222A33F44444444DDCCA1FC33'
    }, function(err, response) {
      // err: failed request or error in TeleSign response
      // response: JSON response from TeleSign
    });

**You have 12 hours to get these results.**

You can have TeleSign push the results to you automatically by subscribing to a [Verify Transaction Callback](http://docs.telesign.com/rest/content/transaction-callback.html#xref-transaction-callback).

### Smart Verify

*Not yet implemented in this module.*

## Mobile Device

*Not yet implemented in this module.*

## TeleBureau

### Submit a TeleBureau Event

The TeleBureau Event web service allows you to programmatically submit a telephone number to TeleSign, as a candidate for addition to our TeleBureau watchlist.

Details on the options for the API to submit a TeleBureau event can be found [here](http://docs.telesign.com/rest/content/telebureau-event.html#request-parameters).

    var options = {
      phoneNumber: '15551234567', // required
      fraudType: 'chargeback' // required, see list at above URL
      occuredAt: new Date(), // required, uses a JavaScript Date object
      discoveredAt: new Date(), // optional, uses a JavaScript Date object
      fraudIp: '', // optional
      impactType: 'revenue_loss', // optional, see list at above URL
      impact: 'medium', // optional, 'low', 'medium', or 'high' depending on dollar amount, see list at above URL for details
      verifiedBy: 'telesign', // optional, either 'telesign' or 'other'
      verifiedAt: new Date() // optional, uses a JavaScript Date object
    };

    telesign.teleBureau.submitEvent(options, function(err, response) {
      // err: failed request or error in TeleSign response
      // response: JSON response from TeleSign
    });

### Get a TeleBureau Event

After you’ve submitted a Fraud Event, you can check to see whether TeleSign accepted it (and therefore added it to the TeleBureau watchlist) by sending a GET request to the resource created.

    telesign.teleBureau.getEvent({
      referenceId: '11111BB222A33F44444444DDCCA1FC33'
    }, function(err, response) {
      // err: failed request or error in TeleSign response
      // response: JSON response from TeleSign
    });

### Delete a TeleBureau Event

After you’ve made your submission request, you can cancel it by sending a DELETE request to the resource indicated by appending the Reference ID returned in response to your submission request.

    telesign.teleBureau.deleteEvent({
      referenceId: '11111BB222A33F44444444DDCCA1FC33'
    }, function(err, response) {
      // err: failed request or error in TeleSign response
      // response: JSON response from TeleSign
    });

[license-image]: http://img.shields.io/badge/license-MIT-blue.svg?style=flat-square
[license-url]: https://github.com/giftnix/telesign/blob/master/LICENSE

[npm-version-image]: http://img.shields.io/npm/v/telesign.svg?style=flat-square
[npm-downloads-image]: http://img.shields.io/npm/dm/telesign.svg?style=flat-square
[npm-url]: https://npmjs.org/package/telesign

[travis-image]: http://img.shields.io/travis/giftnix/telesign.svg?style=flat-square
[travis-url]: http://travis-ci.org/giftnix/telesign