
Cisco Spark - List rooms Python

# Step 1. List Your Spark Rooms
Copy/paste the following code to your script. It imports the required modules

```python
# coding=utf-8
import json
import requests
```

> **NOTE**: The “```# coding=utf-8```” handles special characters in room names

Then copy/paste the Personal Access Token variables and put your token in it.
```python
myToken="YOUR_PERSONAL_ACCESS_TOKEN_HERE"
myToken="Bearer "+myToken
```
The token send to Spark has the format "Bearer XjY3ZMD…", added in the 2nd line above
Add this function (a collection of code that can re-used) that lists all of your rooms when called with 1 parameter:  your Token. 
```python
def get_rooms(mytoken):
	# The header is send to authenticate
	header = {'Authorization':mytoken, 'content-type':'application/json'}
	# Send GET request with above header, put result in 'result'
	result=requests.get(url='https://api.ciscospark.com/v1/rooms',headers=header)
	# Encode 'result' as JSON
	JSONresponse = result.json()
	# Create an Array for all rooms: ('room1','room2','room3')
	roomlist_array = []
	# For each item in the JSON data
	for EachRoom in JSONresponse['items']:
		# add the 'title' + 'room ID' to the roomlist_array
		roomlist_array.append(EachRoom.get('title') +
						' ** ' + EachRoom.get('id'))
	# Return the list of members
	return roomlist_array
```


Sample JSON response after GET request:
```json
{
"items": [{
    "lastActivity": "2016-03-23T09:25:08.217Z",
    "created": "2016-03-22T16:22:50.745Z",
    "isLocked": True,
    "id": "Y2lzY29zcGFyazovL30YS0xMWU1LTlhY2ItZWQwMThkMzRlMDU2",
    "title": "EMEAR Cloud Collab SE"
    }]
}
```
Next add this code that executes the get_rooms function and print a list of my rooms:
```python
# Execute get_rooms and put result in 'SparkResult'
SparkResult = get_rooms(myToken)
# Loop through the SparkResult array
for room in SparkResult:
	# print room name
	print ("> Room:", room)
# Print the number of rooms (length of SparkResult array)
print ("----- TOTAL Room Count:", len(SparkResult))
```

Execute the code by clicking "RUN" and look at the output. It should look something like this: 
```
> Room: API doc team  ** YU21ZiMjYtZjN1ZigxYjDk2TAcGFyMDcx1ZiMWU21ZiMjYtZjN1ZigxYjBmNDk2
> Room: Customer ABC ** YAcGFyMDcx1ZiMWU21ZiMjYtZjN1ZigxYjBmNDk2Y2zcazovcGFyL1JPcGFyTlhO
> Room: Team planning ** Y2l1Zi29zcGFyazovcGFyL1JPcGFyTTcx1ZiMWU21ZiMjYtZjN1ZigxYjBmNDk2
```

> **NOTE** that this code will only return a maximum of 100 rooms. This is the default max of the Spark API unless you specify the Max parameter (see list rooms documentation) 

