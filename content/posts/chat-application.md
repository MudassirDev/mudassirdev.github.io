+++
title = "I tried to build a chat application"
date = "2025-10-06T22:34:56.437Z"
#dateFormat = "2006-01-02" # This value can be configured for per-post date formatting
author = "Mudassir Bilal"
authorTwitter = "" #do not include @
cover = ""
tags = ["", ""]
keywords = ["", ""]
description = ""
showFullContent = false
readingTime = false
hideComments = false
+++
I started working on a personal project, the project is still not complete but what I want to talk about is a project that was created for the main project.

**QUICK NOTE**: I am not going to talk about auth part, that's a talk for another time

So I have this project I wanted to complete but after a certain point I realized that a HUGE part of my project is chatting, if users cannot talk to each other on my app then the whole idea is flawed, so I decided to make a new project for learning purposes.

I had never used websockets before, this was my first time doing real-time communication on the backend, so I got straight to it, since my original project was started in Go I decided to write the chat application in Go as well.

I didn't write the web sockets from scratch because I wanted to finish this project and get back to the original project, I am not trying to write a web sockets library for Go so I decided to use the [Gorilla Websocket](https://github.com/gorilla/websocket) for this.

Ok so since the project was just for learning purposes I chose SQLite DB for this, because it's very simple I could not care less (yet), I already knew how to do auth so I did that part first, I implemented token based authentication and moved on.

So I read the documentation and made my first client to server connection, great! client can now talk to the server but wait, it was not that simple, even though I had a way for the client to talk to the server that was not all that's needed, it was just the beginning.

Now that I had one user talking to the client what about the rest? how would I manage connection for a lot of users? my first idea was to create a simple map of users ID to the connection pointer, I thought about scalibility but worrying about at this point of the project would be futile since I haven't even published yet and nor do I have any users so I went with the idea, YES I know storing a map in the memory might be bad but it works.

Ok so we now I had a way of holding multiple connections to multiple clients, so I made a simple messages table and got to work, I made it so that each user can send a message to other user and it would just forward the message to the second user by using the recipient ID, since I had a map of ID's to connections it worked great and I got my first every message from one client to the second.

Then I decided to save every message, so now the flow was: User A sends a message to User B through the websocket, the JSON contains the recipient ID, server stores the message to the database and if I found a connection for the user B then I would send the message to User B and User A both, but if I didn't find a connection for User B, I would return the message to User A and store it.

After this I whipped a quick UI, honestly I don't have a knack for designing stuff so I had Claude give me a simple HTML and CSS UI to work with, then using JS I wrote the interaction, then I made it so that when you would open a user it would get the messages from the backend and load them in the chatbox.

I now had a flow where user would be able to talk to each other, UI was updating and if the recipient of a message is offline then the message would still be stored and get loaded when the recipient comes online.

Now at this point I decided to migrate to PostgreSQL, why you may ask? it's because I was handling the user ID at the application level since SQLite didn't support it, and it was getting pretty annoying to manage, so I just migrated to PostgreSQL and called it a day.

At this point I was feeling great, but that feeling faded away quickly because my app was just another chat application, even though it was for learning purposes I now decided to add voice messaging to make it a bit unique and also challenge myself.

So I opened MDN Docs and searched on how I can record audio, turns out it's pretty easy, all I had to do was use the MediaRecorder API with a stream, I got the stream through the users device and passed it the MediaRecorder, then after the message was recorder I got the ArrayBuffer from the blob, converted it into a Uint8Array and created a new Array of bytes.

I then modified my message structure a bit to send back either the text message or the bytes for the audio. On the backend I handled the new message structure with a simple message type field, if the message was TEXT type I simply stored it in database, if it was AUDIO type I took the bytes, created a new audio file and stored the file on the server and saved the file URL in the message row.

Now I know that it would be better to user something like S3 to store the file instead of serving it from the server but that was for another time and would be too much for this project.

I finally had a new feature, then I simply implemented notifications on the client side, changed the UI a bit so that users could talk to one other and called it a Finish project.

I have one more feature that I might implement in near future which was message state, because if I didn't have a message state that if the message is read or not then the users wouldn't be able to tell whom's messages they have not read yet but that's for another time.

I am proud of this little creation I made and I enjoyed it a lot. but yeah, let me know what are your thoughts about this project on my discord: `thevecneccy`
