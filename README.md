# REST API Views 
A TrendMiner view consists of one or more layers with one or more tags. The API allows
retrieving all data points from all visible tags in all visible layers of a saved view.
Steps to retrieve render data for TrendMiner views:
- Request an access token for authentication
- Retrieve the View ID
- Retrieve the view data by view ID

# Authentication
Currently, only OAuth2 authentication is supported.
An access token can be requested with the TrendMiner security API.

**Method** :  POST

**Path** : /security/oauth/token

**Authorization header** : Basic
dHJlbmRtaW5lckFwaTpjdmJwa3R6SmVGUENLVk10dFN0ZVQ1ejNlYXlkeUZaYQ==

**Body**
grant_type: password
username: [TrendMiner username]
password: [TrendMiner password]

# API Views 
A swagger interface is shipped with your appliance and is available on /work/swagger-ui.html
To retrieve the ID of a view, search for it by name.

**Method** : GET

**Path** : /work/saveditem/search?query=[view name]

**Authorization header** : Bearer [access_token]

**Example:**
```javascript
/work/saveditem/search?query=demo
```
The result will contain all matching views. Copy the value of the 'identifier' field of the view for
which you want to retrieve the data.
Use the render function to retrieve the data points of the view.

**Method** : GET

**Path** : /work/{id}/render

**Authorization header** : Bearer [access_token]

**Parameters** :
**start** : Optional. ISO Timestamp Format, YYYY-MM-DDTHH:mm:ssZ
**end** : Optional, only valid if also start is provided . ISO Timestamp Format,
YYYY-MM-DDTHH:mm:ssZ
**interval_seconds** : Optional. The number of seconds to use for each interval, defines the
granularity of the data.

Example 1:
**Example:**
```javascript
/work/7/render
```
Example 2:
```javascript
/work/7/render?start=2015-03-04T10:00:00.000Z&end=2015-03-06T10:00
:00.000Z&interval_seconds=3600
```

# Important notes
- The access token will expire after 12 hours. The best practice is to request an access
token before every call another API call.
- Only index data is returned by the render function. In case the granularity of the data
is too high for the requested time period, the render function will return no data points
and will not return any warning or error message.
- To prevent that the response of the render function overflows the client for large data
sets, the response will be offered to the client in chunks.
