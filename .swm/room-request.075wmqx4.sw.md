---
title: Room request
---
# Introduction

This document will walk you through the implementation of the "Room request" feature.

The feature allows users to join or create chat rooms by specifying a room name and username.

We will cover:

1. URL routing for room requests.
2. Handling room requests in views.
3. Room model definition.

# URL routing for room requests

<SwmSnippet path="/chating/urls.py" line="6">

---

We define the URL pattern for accessing a specific chat room. This pattern captures the room name from the URL and passes it to the view.

```
    path('<str:room>/', views.room, name="room"),
```

---

</SwmSnippet>

# Handling room requests in views

## Room view

<SwmSnippet path="/chating/views.py" line="9">

---

The <SwmToken path="/chating/views.py" pos="10:2:2" line-data="def room(request, room):">`room`</SwmToken> view handles rendering the chat room page. It retrieves the username from the query parameters and fetches the room details from the database.

```

def room(request, room):
    username = request.GET.get('username')  # henry
    room_details = Room.objects.get(name=room)
    return render(request, 'room.html', {

        'username': username,
        'room': room,
        'room_details': room_details,
    })
```

---

</SwmSnippet>

## Checkview

<SwmSnippet path="/chating/views.py" line="21">

---

The <SwmToken path="/chating/views.py" pos="21:2:2" line-data="def checkview(request):">`checkview`</SwmToken> function handles the logic for checking if a room exists or creating a new one if it doesn't. It then redirects the user to the appropriate room URL with the username as a query parameter.

```
def checkview(request):
    room = request.POST['room_name']
    username = request.POST['username']

    if Room.objects.filter(name=room).exists():
        return redirect('/'+room+'/?username='+username)
    else:
        new_room = Room.objects.create(name=room)
        new_room.save()
        return redirect('/'+room+'/?username='+username)
```

---

</SwmSnippet>

# Room model definition

<SwmSnippet path="/chating/models.py" line="5">

---

The <SwmToken path="/chating/models.py" pos="5:2:2" line-data="class Room(models.Model):">`Room`</SwmToken> model defines the structure of a chat room in the database. It includes a single field for the room name.

```
class Room(models.Model):
    name = models.CharField(max_length=1000)
```

---

</SwmSnippet>

This concludes the walkthrough of the "Room request" feature. The implementation ensures that users can join existing rooms or create new ones seamlessly.

&nbsp;

```mermaid

```

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBTG92ZUNoYXQlM0ElM0FnaWxhZG5hdm90" repo-name="LoveChat"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>
