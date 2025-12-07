FAST-NUCES Campus Messaging System
Video Demonstration Script
Total Duration: 8-10 minutes
Setup: 3 terminal windows (1 Server + 2 Clients) visible on screen

SETUP INSTRUCTIONS (Before Recording)
Terminal Layout:
┌─────────────────────┬─────────────────────┐
│                     │                     │
│   Server Terminal   │  Client 1 Terminal  │
│   (Top/Left)        │  (Top/Right)        │
│                     │                     │
├─────────────────────┴─────────────────────┤
│                                           │
│         Client 2 Terminal                 │
│         (Bottom - Full Width)             │
│                                           │
└───────────────────────────────────────────┘
Pre-Recording Checklist:
 Compile both programs: gcc server.c -o server -lpthread and gcc client.c -o client -lpthread
 Test your microphone audio quality
 Increase terminal font size for better visibility (14-16pt)
 Close unnecessary applications
 Have credentials ready:
Lahore: NU-LHR-123
Karachi: NU-KHI-123
Peshawar: NU-PSH-123
VIDEO SCRIPT
[0:00 - 0:30] INTRODUCTION
[Show desktop with 3 empty terminals]

Speaker 1:

"Assalam-o-Alaikum everyone. I'm [Name], and today we'll be demonstrating our Computer Networks Lab project - the FAST-NUCES Multi-Campus Communication System."

Speaker 2:

"This project implements a client-server architecture using hybrid TCP/UDP socket programming in C. We'll show you how multiple campuses can communicate through a central server, just like FAST-NUCES's real multi-campus structure."

Speaker 1:

"Let's start by explaining what you're seeing on the screen. We have three terminal windows: one for our Central Server, and two for Campus Clients representing Lahore and Karachi campuses."

[0:30 - 1:30] STARTING THE SERVER
[Focus on Server Terminal - Top Left]

Speaker 2:

"First, we need to start our central server. This server will act as the communication hub for all campuses. Let me run the server executable."

[Type in Server Terminal:]

bash
./server
[Server Output appears:]

[SERVER] TCP listening on port 5000
[SERVER] UDP listening on port 6000
[SERVER] Admin console ready. Type 'list' or 'broadcast <message>'
Speaker 1:

"Perfect! As you can see, our server is now running. Notice three important things here:"

[Pause and point to each line]

Speaker 1:

"First, TCP port 5000 is listening - this handles reliable communication like authentication and direct messaging. Second, UDP port 6000 is ready - this handles heartbeats and broadcasts. And third, the admin console is active, which we'll use later to monitor connections and send announcements."

Speaker 2:

"Let me briefly explain how this works in the code. In our main function, we create two separate sockets - one TCP socket using SOCK_STREAM for reliable connections, and one UDP socket using SOCK_DGRAM for fast broadcasts. We bind them to different ports and start three separate threads."

[Optional: Show code snippet on screen or in a text editor]

Speaker 2:

"The first thread runs the UDP listener function, which continuously receives heartbeat packets. The second thread runs the admin console, processing commands like 'list' and 'broadcast'. And the main thread uses an accept loop to handle incoming TCP connections from campus clients. This multi-threaded design allows our server to handle multiple operations simultaneously."

Speaker 1:

"The server is now waiting for campus clients to connect. Let's connect our first campus - Lahore."

[1:30 - 3:00] CONNECTING FIRST CLIENT (LAHORE)
[Focus on Client 1 Terminal - Top Right]

Speaker 1:

"Now we'll start our first campus client. This represents the Lahore campus Admissions department."

[Type in Client 1 Terminal:]

bash
./client
[Client Output:]

===== FAST-NUCES Campus Client =====
Enter Campus Name (e.g., Lahore):
Speaker 1: [Type: Lahore]

"I'm entering 'Lahore' as the campus name."

[Client Output:]

Select Department:
1. Admissions
2. Academics
3. IT
4. Sports
Choice (1-4):
Speaker 1: [Type: 1]

"And selecting option 1 for the Admissions department."

[Client Output:]

Enter Password for Lahore:
Speaker 1: [Type: NU-LHR-123]

"Each campus has a unique password for authentication. For Lahore, it's NU-LHR-123."

[Client Output:]

Connected and authenticated as Lahore - Admissions Department
Instructions:
- To send message: TargetCampus,TargetDept,Message
- Example: Karachi,IT,Hello from Lahore Admissions
- Departments: Admissions, Academics, IT, Sports

===== Lahore Campus - Admissions Department =====
1. Send message to another campus
2. View message history
3. Check online campuses (from server)
4. Exit
Choice:
Speaker 2:

"Excellent! Lahore has successfully connected. Notice the menu with four options. But more importantly, look at the server terminal."

[Switch focus to Server Terminal]

[Server Output shows:]

[SERVER] New TCP client connected, awaiting credentials...
[SERVER] Lahore Admissions authenticated and TCP session started.
Speaker 2:

"The server has recognized the connection, validated the credentials, and started a TCP session. Let me explain what happened behind the scenes in the code."

Speaker 1:

"When the client connects, it sends credentials in the format Campus:Department:Password. Our server reads this using the read system call, then parses it by finding the colon delimiters using the strchr function. The campus name and password are extracted and passed to our authenticate function."

Speaker 2:

"The authenticate function loops through a hardcoded array of valid credentials - comparing the provided campus name and password with stored values. If authentication succeeds, the server stores the client's socket descriptor, campus name, and department in parallel arrays, then sends back 'AUTH_OK'. After that, it creates a new thread using pthread_create to handle all future messages from this client."

Speaker 1:

"This threading approach is crucial - it allows the server to accept new connections while simultaneously handling messages from already connected clients. Each client gets its own dedicated thread running the clientHandler function."

[After 10 seconds, Server shows:]

[UDP][HEARTBEAT] Lahore Admissions (stored UDP addr). LastSeen updated.
Speaker 1:

"And there's the first heartbeat! Every 10 seconds, the client sends a UDP heartbeat to inform the server it's still online. This is our UDP status monitoring in action."

Speaker 2:

"In the client code, we have a separate thread called udpHeartbeat that runs an infinite loop. Every 10 seconds, it uses sendto to send the campus name and department to the server's UDP port. The server's UDP listener thread receives this with recvfrom, parses the campus and department, then updates the lastSeen timestamp and stores the client's UDP address for future broadcasts."

[3:00 - 4:30] CONNECTING SECOND CLIENT (KARACHI)
[Focus on Client 2 Terminal - Bottom]

Speaker 2:

"Let's connect a second campus - Karachi IT department. This will demonstrate how our server handles multiple concurrent connections."

[Type in Client 2 Terminal:]

bash
./client
[Follow same process:]

Campus Name: Karachi
Department: 3 (IT)
Password: NU-KHI-123
Speaker 2:

"I'm entering Karachi, selecting IT department - option 3, and providing the Karachi campus password."

[Client connects successfully]

Speaker 1:

"Now we have two campuses connected simultaneously. Let's verify this using the admin console."

[Switch to Server Terminal]

[Server shows both connections and heartbeats]

Speaker 1: [Type in Server Terminal:]

bash
list
[Server Output:]

---- Connected campuses (2) ----
1) Lahore | Dept: Admissions | TCPFD=4 | UDP known=1 | lastSeen=2024-12-07 14:30:25
2) Karachi | Dept: IT | TCPFD=5 | UDP known=1 | lastSeen=2024-12-07 14:30:23
------------------------------
Speaker 2:

"Perfect! The 'list' command shows both campuses are connected. You can see their department names, TCP file descriptors, and most importantly - their last-seen timestamps from UDP heartbeats. This demonstrates our server's ability to handle multiple concurrent TCP connections and track client status via UDP."

[4:30 - 6:00] DEMONSTRATING MESSAGE ROUTING
[Focus on Client 1 - Lahore]

Speaker 1:

"Now let's demonstrate the core feature - inter-campus messaging. I'll send a message from Lahore Admissions to Karachi IT."

[In Lahore Terminal:]

Choice: 1

Enter message (TargetCampus,TargetDept,Message):
>
Speaker 1: [Type:]

Karachi,IT,Hello from Lahore Admissions! We need IT support for new student registrations.
[Press Enter]

Message sent.
Speaker 1:

"I've sent the message using our custom protocol format: Target Campus comma Target Department comma Message content."

[Switch to Server Terminal]

[Server Output:]

[TCP][Lahore Admissions] >> Karachi,IT,Hello from Lahore Admissions! We need IT support for new student registrations.
[SERVER] Routed message from Lahore Admissions to Karachi IT.
Speaker 2:

"Look at the server terminal. It received the message via TCP, parsed the destination, and successfully routed it to Karachi IT. This is our message routing mechanism in action."

[Switch to Client 2 - Karachi]

[Karachi Terminal automatically shows:]

[MSG] [Lahore Admissions -> Karachi IT] Hello from Lahore Admissions! We need IT support for new student registrations.
Speaker 2:

"And here's the message received by Karachi IT! Notice how the server added the sender information in the format. The entire routing happened seamlessly through our TCP connection."

[6:00 - 7:00] DEMONSTRATING MESSAGE HISTORY AND RESPONSE
[Focus on Client 2 - Karachi]

Speaker 1:

"Let's have Karachi respond, and then check the message history feature."

[In Karachi Terminal:]

Choice: 1

Enter message (TargetCampus,TargetDept,Message):
> Lahore,Admissions,Sure! We'll send our team over tomorrow morning.
Message sent.
[Switch to Lahore Terminal - message appears automatically]

[MSG] [Karachi IT -> Lahore Admissions] Sure! We'll send our team over tomorrow morning.
Speaker 2:

"Message delivered successfully! Now let's check Lahore's message history."

[In Lahore Terminal:]

Choice: 2
[Output:]

===== MESSAGE HISTORY (2 messages) =====
1. [Karachi IT -> Lahore Admissions] Sure! We'll send our team over tomorrow morning.
2. [Lahore Admissions -> Karachi IT] Hello from Lahore Admissions! We need IT support for new student registrations.
=====================================
Speaker 2:

"Perfect! The message history shows both sent and received messages, making it easy to track all communications. This feature stores up to 50 messages per client."

[7:00 - 8:00] DEMONSTRATING UDP BROADCAST
[Focus on Server Terminal]

Speaker 1:

"Now let's demonstrate the UDP broadcast feature. The admin can send announcements to all connected campuses simultaneously."

[Type in Server Terminal:]

bash
broadcast Attention all campuses: System maintenance scheduled for tonight at 10 PM. Please save your work.
[Server Output:]

[ADMIN] Broadcast sent to 2 clients: Attention all campuses: System maintenance scheduled for tonight at 10 PM. Please save your work.
[Switch to Client 1 - Lahore]

[Immediately shows:]

[ADMIN BROADCAST] Attention all campuses: System maintenance scheduled for tonight at 10 PM. Please save your work.
[Switch to Client 2 - Karachi]

[Shows same message:]

[ADMIN BROADCAST] Attention all campuses: System maintenance scheduled for tonight at 10 PM. Please save your work.
Speaker 2:

"Excellent! Both clients received the broadcast instantly via UDP. This demonstrates how we use UDP for non-critical announcements that need to reach everyone quickly without requiring acknowledgment."

[8:00 - 8:30] DEMONSTRATING ONLINE STATUS CHECK
[Focus on Client 1 - Lahore]

Speaker 1:

"Clients can also check which campuses are currently online. Let me demonstrate."

[In Lahore Terminal:]

Choice: 3
Request sent to server. Check received messages.
[After a moment:]

[MSG] [SERVER] Connected Campuses:
  1. Lahore - Admissions (Last seen: 14:35:10)
  2. Karachi - IT (Last seen: 14:35:08)
----------------------------
Speaker 1:

"Perfect! The client received a list of all connected campuses with their last-seen times from UDP heartbeats. This helps departments know who's available for communication."

[8:30 - 9:00] DEMONSTRATING GRACEFUL DISCONNECTION
[Focus on Client 2 - Karachi]

Speaker 2:

"Finally, let's show what happens when a client disconnects."

[In Karachi Terminal:]

Choice: 4
Exiting...
[Terminal closes or returns to prompt]

[Switch to Server Terminal]

[Server Output:]

[SERVER] Karachi IT disconnected or socket closed.
Speaker 2:

"The server detected the disconnection and gracefully cleaned up the connection. Let's verify with the list command."

[Type in Server:]

bash
list
[Output:]

---- Connected campuses (1) ----
1) Lahore | Dept: Admissions | TCPFD=4 | UDP known=1 | lastSeen=2024-12-07 14:36:15
------------------------------
Speaker 2:

"Now only Lahore remains in the connected list. Our server properly manages client connections and disconnections."

[9:00 - 9:45] HIGHLIGHTING KEY FEATURES
[Show all three terminals]

Speaker 1:

"Let me summarize what we've demonstrated today:"

[Speak clearly while listing]

Speaker 1:

"One: TCP Authentication - Secure credential validation when clients connect."

"Two: Concurrent Connections - Server handles multiple campus clients using threading."

"Three: Message Routing - TCP-based reliable routing from source to destination departments."

"Four: UDP Heartbeats - Every 10 seconds, clients send status updates to maintain online presence."

"Five: UDP Broadcasts - Admin can send announcements to all campuses instantly."

"Six: Message History - Clients maintain a record of all communications."

"Seven: Online Status - Clients can query the server for currently connected campuses."

Speaker 2:

"Our implementation successfully demonstrates the hybrid TCP/UDP approach required by the project specifications. TCP provides reliability for critical operations, while UDP offers efficiency for status updates and broadcasts."

[9:45 - 10:00] CONCLUSION
[Show terminal windows or return to desktop]

Speaker 1:

"This project demonstrates core concepts of network programming including socket creation, client-server architecture, protocol design, concurrency using threads, and the strategic use of TCP versus UDP."

Speaker 2:

"Thank you for watching our demonstration. We hope this clearly showed how our multi-campus communication system works. The source code and technical documentation are available in our project submission."

Speaker 1:

"Thank you!"

[Fade out]

POST-RECORDING CHECKLIST
 Video length is 8-10 minutes
 Audio is clear and audible
 All terminal text is readable
 All required features demonstrated:
 TCP authentication
 UDP heartbeats (visible in server logs)
 Message routing between clients
 Admin broadcast
 Server concurrency (2 clients)
 Graceful disconnection
 Voice-over explains each step clearly
 No dead silence or awkward pauses
 Video ends professionally
TIPS FOR RECORDING
Technical Tips:
Screen Recording Software: Use OBS Studio (free) or ShareX
Audio: Use a decent microphone, not laptop mic if possible
Font Size: Make terminal font at least 14-16pt
Recording Quality: 1080p at minimum, 30fps
Practice: Do a dry run before final recording
Presentation Tips:
Speak Clearly: Pronounce technical terms carefully
Pace: Don't rush, give viewers time to read output
Enthusiasm: Show confidence in your work
Professionalism: Avoid "umm," "like," filler words
Coordination: If two speakers, transition smoothly between them
Common Mistakes to Avoid:
❌ Typing too fast - viewers can't follow
❌ Not waiting for output to display
❌ Mumbling or speaking too quietly
❌ Not explaining what you're doing
❌ Skipping the heartbeat demonstration
❌ Forgetting to show server logs during messaging
ALTERNATIVE: SOLO RECORDING
If recording alone, simply use "I" instead of alternating speakers:

"Hello everyone, I'm [Name], and today I'll demonstrate..."
"Now I'll start the server..."
"Let me connect the first client..."

The script structure remains the same!

Good luck with your recording! 🎥

