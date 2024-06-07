---
layout: default
title: Scripting API
nav_order: 11
permalink: /scripting-api
---

# Scripting API
{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

## Preamble

### Definitions
- **Datum**: A file, folder or task to be organized by Ritt

### API usage
Ritt APIs are called by sending JSON-formatted strings, called Commands, to localhost. To enable scripting, go to Settings -> Scripting -> Enable Scripting. The default address and port is ```127.0.0.1:52848```. You can change the port number from Settings -> Scripting -> Port number. The response is also in the form of a JSON-formatted string. To authenticate a Command, an access key is required. This can be obtained from Settings -> Scripting -> Enable Scripting. In this documentation, we will use Python to call Ritt APIs, although other languages can used as well.

### Sending commands (Python example)
```python
import socket, json, struct

# Set the IP address and port to connect to
ip_address = "127.0.0.1"
port = 52848

# Create a socket object
client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Connect to the server
try:
	client_socket.connect((ip_address, port))
	print("Connected to Ritt")
except Exception as e:
	# Handle any exception
	print("An error occurred:", e)

def sendCommand(cmd): 
    # Prefix each message with a 4-byte length (native byte order)
    msg = json.dumps(cmd).encode('utf-8')
    msg = struct.pack('@i', len(msg)) + msg
    # convert to json str and send through socket
    client_socket.sendall(msg)

    # Receive response from Ritt
    # Read message length and unpack it into an integer
    raw_msglen = recvall(client_socket, 4)
    if not raw_msglen:
        return None
        
    msglen = struct.unpack('@i', raw_msglen)[0]

    # Read the message data
    received_data = recvall(client_socket, msglen)
    response = received_data.decode('utf-8')
    print(response)

def recvall(sock, n):
    # Response can potentially be very long, and might be carried by different packets
	# ref: https://stackoverflow.com/questions/17667903/python-socket-receive-large-amount-of-data
    data = bytearray()
    while len(data) < n:
        packet = sock.recv(n - len(data))
        if not packet:
            return None
        data.extend(packet)
    return data
```
		

### Closing the connection
```python
client_socket.close()
```


## APIs

### Get Tag Root
Get the name and ID of the root tag.

```python
command = {
    "AccessKey": "get-key-from-settings-scripting",
	"Command":"GetTagRoot"
}
sendCommand(command)
```

**Response**
The name and ID of the root tag

```json
{
  "Command": "GetTagRoot",
  "Response": "OK",
  "Result": {
    "Name": "🏷",
    "ID": 2
  }
}
```

### Get Children Tags

Get the names and IDs of all children tags of a tag, to be located by ID. This can be called repeatedly to iterate the tag tree.

**Required parameters**
- ```TagID```: ID of the tag of interest 


```python
command = {
    "AccessKey": "get-key-from-settings-scripting",
    "Command": "GetChildrenTags",
    "Parameters": 
    {
        "TagID": 22,
    }
}
sendCommand(command)
```		

**Response**
The names and IDs of all children tags

```json
{
  "Command": "GetChildrenTags",
  "Response": "OK",
  "Result": [
    {
      "Name": "Alpha",
      "ID": 31
    },
    {
      "Name": "Bravo",
      "ID": 243
    },
    {
      "Name": "Charlie",
      "ID": 244
    }
  ]
}
```

### Get Parent Tag

Get the name and ID of the parent tag of a tag, to be located by ID.

**Required parameters**
- ```TagID```: ID of the tag of interest 


```python
command = {
    "AccessKey": "get-key-from-settings-scripting",
    "Command": "GetParentTag",
    "Parameters": 
    {
        "TagID": 22,
    }
}
sendCommand(command)
```		

**Response**
The name and ID of the parent tag

```json
{
  "Command": "GetParentTag",
  "Response": "OK",
  "Result": {
    "Name": "Tags for testing",
    "ID": 266
  }
}
```

### Get All Tags

Get all the tags in the database.

```python
command = {
    "AccessKey": "get-key-from-settings-scripting",
    "Command": "GetAllTags"
}
sendCommand(command)
```

**Response**
The names and IDs of all tags

```json
{
  "Command": "GetAllTags",
  "Response": "OK",
  "Result": [
    {
      "Name": "Tag1",
      "ID": 545
    },
    {
      "Name": "Tag2",
      "ID": 698
    }
  ]
}
```


### Get Tag Name

Get the name of a tag, specified by ID.

**Required parameters**
- ```TagID```: ID of the tag of interest 


```python
command = {
    "AccessKey": "get-key-from-settings-scripting",
    "Command": "GetTagName",
    "Parameters": 
    {
        "TagID": 244,
    }
}
sendCommand(command)
```		

**Response**
The name and ID of the tag

```json
{
  "Command": "GetTagName",
  "Response": "OK",
  "Result": "Charlie"
}
```

### Create tags

Create new tags under a parent tag.

**Required parameters**
- ```ParentTagID```: ID of the parent tag
- ```TagNames```: Names of the all the children tags to be created

```python
command = {
    "AccessKey": "get-key-from-settings-scripting",
    "Command": "CreateTags",
    "Parameters": 
    {
        "ParentTagID": 22,
        "TagNames": ["Delta", "Echo"]
    }
}
sendCommand(command)
```

**Response**
The names and IDs of newly created tags

```json
{
  "Command": "CreateTags",
  "Response": "OK",
  "Result": [
    {
      "Name": "Delta",
      "ID": 196
    },
    {
      "Name": "Echo",
      "ID": 198
    }
  ]
}
```

### Delete tags

Delete tags by ID

**Required parameters**
- ```TagIDs```: IDs of the target tags

```python
command = {
    "AccessKey": "get-key-from-settings-scripting",
    "Command": "DeleteTags",
    "Parameters": 
    {
        "TagIDs": [196, 198]
    }
}
sendCommand(command)
```

**Response**
```json
{
  "Command": "DeleteTags",
  "Response": "OK"
}
```

### Rename tag

Rename a tag

**Required parameters**
- ```TagID```: ID of the target tag
- ```NewName```: New name

```python
command = {
    "AccessKey": "get-key-from-settings-scripting",
    "Command": "RenameTag",
    "Parameters": 
    {
        "TagID": 219,
        "NewName": "Foxtrot"
    }
}
sendCommand(command)
```

**Response**
```json
{
  "Command": "RenameTag",
  "Response": "OK"
}
```

### Get Tag Alias

Get the alias of a tag

**Required parameters**
- ```TagID```: ID of the target tag

```python
command = {
    "AccessKey": "get-key-from-settings-scripting",
    "Command": "GetTagAlias",
    "Parameters": 
    {
        "TagID": 673
    }
}
sendCommand(command)
```

**Response**
```json
{
  "Command": "GetTagAlias",
  "Response": "OK",
  "Result": "Some alias"
}
```

### Set Tag Alias

Set the alias of a tag

**Required parameters**
- ```TagID```: ID of the target tag
- ```NewAlias```: New alias

```python
command = {
    "AccessKey": "get-key-from-settings-scripting",
    "Command": "SetTagAlias",
    "Parameters": 
    {
        "TagID": 673,        
        "NewAlias": "Some other alias"
    }
}
sendCommand(command)
```

**Response**
```json
{
  "Command": "SetTagAlias",
  "Response": "OK"
}
```

### Duplicate tags

Duplicate tags by ID

**Required parameters**
- ```TagIDs```: IDs of the target tags

```python
command = {
    "AccessKey": "get-key-from-settings-scripting",
    "Command": "DuplicateTags",
    "Parameters": 
    {
        "TagIDs": [219, 224]
    }
}
sendCommand(command)
```

**Response**
Names and IDs of the duplicated tags
```json
{
  "Command": "DuplicateTags",
  "Response": "OK",
  "Result": [
    {
      "Name": "Foxtrot",
      "ID": 226
    },
    {
      "Name": "Echo",
      "ID": 236
    }
  ]
}
```

### Move tags

Moves tags to a new parent tag

**Required parameters**
- ```TagIDs```: IDs of the target tags
- ```DestinationID```: ID of the new parent tag

```python
command = {
    "AccessKey": "get-key-from-settings-scripting",
    "Command": "MoveTags",
    "Parameters": 
    {
        "TagIDs": [196, 198],
		"DestinationID": 266,
    }
}
sendCommand(command)
```

**Response**
Names and IDs of successfully moved tags
```json
{
  "Command": "MoveTags",
  "Response": "OK",
  "Result": [
    {
      "Name": "Delta",
      "ID": 196
    },
    {
      "Name": "Echo",
      "ID": 198
    }
  ]
}
```


### Tagging

Apply tags to datums, located by either path (file or folder) or ID (file, folder or task). Invalid DatumPaths, DatumIDs or TagIDs will generate warning.

**Optional parameters**
- ```DatumPaths```: Absolute paths of the files/folders to be tagged
- ```DatumIDs```: IDs of datums to be tagged
Specify at least one of the above parameters.

**Required parameters**
- ```TagIDs```: IDs of tags to be applied

```python
sendCommand(command)
command = {
    "AccessKey": "get-key-from-settings-scripting",
    "Command": "Tag",
    "Parameters": 
    {
        "DatumPaths": [r"full\path\to\datum\one", r"full\path\to\datum\two"],
		"DatumIDs": [321],
        "TagIDs": [22,31]
    }
}
sendCommand(command)
```

**Response**
The names, IDs and paths (if applicable) of successfully tagged datums
```json
{
  "Command": "Tag",
  "Response": "OK",
  "Result": [
    {
      "Name": "one",
      "ID": 224,
      "Path": "full\\path\\to\\datum\\one"
    },
    {
      "Name": "two",
      "ID": 236,
      "Path": "full\\path\\to\\datum\\two"
    },
    {
      "Name": "Task A",
      "ID": 321
    }
  ]
}
```

### Untagging
Removing tags from datums. Invalid DatumPaths, DatumIDs or TagIDs will generate warning.

**Optional parameters**
- ```DatumPaths```: Absolute paths of the files/folders to be tagged
- ```DatumIDs```: IDs of datums to be tagged
Specify at least one of the above parameters.

**Required parameters**
- ```TagIDs```: IDs of tags to be applied

```python
sendCommand(command)
command = {
    "AccessKey": "get-key-from-settings-scripting",
    "Command": "Untag",
    "Parameters": 
    {
        "DatumPaths": [r"full\path\to\datum\one", r"full\path\to\datum\two"],
		"DatumIDs": [321],
        "TagIDs": [22,31]
    }
}
sendCommand(command)
```

**Response**
The names, IDs and paths (if applicable) of successfully untagged datums
```json
{
  "Command": "Untag",
  "Response": "OK",
  "Result": [
    {
      "Name": "one",
      "ID": 224,
      "Path": "full\\path\\to\\datum\\one"
    },
    {
      "Name": "two",
      "ID": 236,
      "Path": "full\\path\\to\\datum\\two"
    },
    {
      "Name": "Task A",
      "ID": 321
    }
  ]
}
```

### Get Datums by Tag

Get all datums associated with a tag. This includes:
- Datums directly tagged by this tag
- Datums of children tags if this tags has the attribute Dashboard
- Integrated children datums

**Required parameters**
- ```TagID```: ID of the target tag

```python
command = {
    "AccessKey": "get-key-from-settings-scripting",
    "Command": "GetDatumsByTag",
    "Parameters": 
    {
        "TagID": 31
    }
}
sendCommand(command)
```

**Response**
Name, ID and Path (if is file/folder) of all associated datums

```json
{
  "Command": "GetDatumsByTag",
  "Response": "OK",
  "Result": [
    {
      "Name": "item1",
      "ID": 145,
      "Path": "path\\to\\item1"
    },
    {
      "Name": "item2",
      "ID": 119,
      "Path": "path\\to\\item2"
    }
  ]
}
```
