# Trivia Game

## Project Overview
This Trivia Game is a networked quiz game centered around military themes, implemented in Python using a client-server architecture. Players connect as units of an army, each represented by a real or bot player. Real players are assigned a specific unit and battalion upon joining, while bots are given the name of a famous military commander. The game offers an engaging way to learn about military history and tactics through interactive trivia questions related to army issues.

## Features
- **Military-Themed Trivia**: All questions are related to military topics
- **Hybrid Network Protocol**: Utilizes UDP for discovering clients on the same network and then switches to TCP for reliable, ordered, and error-checked transmission of game data.
- **Concurrency with Threads**: Employs threading to handle multiple client connections in parallel, allowing for real-time interaction without blocking server responsiveness.
- **Event-Driven Logic**: Uses events rather than sleep calls to manage state changes and timing, which optimizes performance and responsiveness by reducing idle time and improving resource utilization.
- **Robust Error Handling**: Implements mechanisms to handle potential errors such as server crashes or client disconnections gracefully, ensuring the game can continue smoothly or terminate cleanly as needed.

## Technical Implementation

### Networking
- **UDP Broadcast**: At first, the server broadcasts a message via UDP to all clients on the same network, inviting them to join the game, in addition we will not close the connection because if there is a client who wants to enter during a game we will not be "lost" and so he can join after the current game will be finished.
- **TCP Connections**: Once clients respond to the UDP broadcast, a TCP connection is established for each client to handle the game's interactive session securely and reliably.

### Multithreading

- **Thread Per Client**: Each client connection is managed by a dedicated thread. This design ensures that client-specific tasks such as receiving answers, timing responses, and sending questions are handled independently, thus avoiding any performance bottlenecks that would arise from a single-threaded server architecture.
  
- **Concurrency Control**: To manage the complexities introduced by multiple threads, the server implements synchronization mechanisms. These include locks and thread-safe data structures to prevent race conditions, especially when accessing shared resources like game state information and player scores.

- **Efficient Resource Management**: Threads are carefully managed to ensure that system resources are utilized optimally. The server dynamically creates threads as clients connect and properly closes these threads when clients disconnect, ensuring efficient resource management and preventing resource leaks.


### Concurrency and Thread Safety
Threads manage client connections, with synchronization mechanisms in place to avoid data races and ensure consistency.

### Event-Driven Architecture
- **Efficient Event Handling**: Instead of using sleep intervals, the server and clients use event signaling for activities such as timing out or proceeding to the next question, which helps in maintaining a smooth and efficient flow of the game mechanics.

### Error Handling
- **Connection Crashes**: The system is designed to detect and handle crashes in both server and client connections. If a client crashes or disconnects unexpectedly, the server logs the event and continues the game with the remaining clients.
- **Timeout Management**: The server implements timeout mechanisms to handle scenarios where a client does not respond within a set period. This ensures that the game does not hang and can proceed to the next question or round.

### Joining the Game
- **Units and Battalions**: Upon connection, each player is assigned a unit and battalion name. Bots receive a random military commander's name.
- **Question Handling**: Players answer trivia questions related to military history and tactics.

### Rules and Scoring
- **Disqualification for Incorrect Answers**: Players who answer incorrectly are disqualified from further participation in the current game but remain connected, because at the end of the game they will be able to receive a message from the winning player
- **Broadcast of Results**: At the game's end, a broadcast message is sent to all participants, announcing the winner and summarizing the game results.


### Performance Metrics
- **Fastest Answers**: The server tracks and stores statistics on who answers each question the fastest, providing a historical record of the fastest responses across all game sessions.

### Game Analysis
- **Difficulty Assessment**: After each game, the server analyzes which questions were answered incorrectly most often, identifying them as the most difficult questions for that session. This analysis helps in adjusting the difficulty level and improving the question set for future games.

