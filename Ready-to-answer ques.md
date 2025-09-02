What is OOP:
-> View entities as objects and info related to
-> Mention and briefly explain Inheritance Polymorph - etc

#### Internship Experience:
###### Frontend Bug Example
**Bug**: Shopping cart quantity update causing page refresh and loss of user selections

**Scenario**: When customers tried to update the quantity of items in their gift cart using the quantity input field, the entire page would refresh instead of updating dynamically. This caused users to lose any personalized gift messages they had typed, selected gift wrapping options, or delivery preferences they had configured.

**Root Cause**: The quantity update button was triggering a full form submission instead of an AJAX call.
**Solution**:
- Implemented client-side JavaScript with AJAX calls to update cart quantities asynchronously
- Added proper event handling to prevent default form submission
- Included loading indicators and error handling for better user experience
- Preserved all user inputs during quantity updates

---
###### Backend Bug Example
**Bug**: Gift card balance not updating after purchase redemption

**Scenario**: When customers applied gift cards during checkout on the e-commerce site, the payment would process successfully and the order would complete, but the gift card balance in the database remained unchanged. This meant customers could reuse the same gift card multiple times.

**Root Cause**: The gift card balance update query was placed after the order completion redirect in the C# controller method, so it never executed due to the early return statement.

**Solution**:
- Moved the gift card balance update logic before the order confirmation step
- Wrapped the payment processing and balance update in a SQL transaction to ensure both operations succeed or fail together
- Added proper error handling to rollback the transaction if the balance update failed