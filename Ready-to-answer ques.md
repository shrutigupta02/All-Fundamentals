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