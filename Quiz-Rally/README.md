# Quiz Rally

**A live multiplayer trivia game built through AI-assisted development.**

[Play Quiz Rally](https://quiz-rally-ayobami.sturdycrumb512.chatgpt.site)

## Overview

Quiz Rally brings 2–8 players together in a shared trivia room. Each player uses their own device, joins with a six-character room code, and competes for points by answering questions accurately and quickly.

The first version was designed around a four-player game night and published on October 25, 2026.

## Features

- Create or join a room using a room code.
- Choose a nickname and mark yourself ready.
- Play ten multiple-choice questions from a mixed-category question bank.
- Answer within a shared 15-second window.
- Lock answers immediately after submission.
- See the correct answer, a short explanation, and updated scores after each round.
- Finish with a winner announcement and final standings.
- Start another game with a new selection of questions.
- Use a responsive interface on desktop or mobile.

## How to play

1. Open the live game and enter a nickname.
2. Create a room and share the game URL and room code.
3. Other players open the same URL and join with the code.
4. Once at least two players have joined and everyone is ready, the host starts.
5. Select one answer per question. Answers cannot be changed after submission.
6. The answer is revealed when everyone has answered or time expires.
7. The host advances to the next question.
8. After ten questions, the player with the most points wins. Tied leaders share the win.

### Scoring

| Result | Points |
| --- | --- |
| Correct answer | 100 |
| Speed bonus for a correct answer | Up to 50 additional points |
| Incorrect or missed answer | 0 |

The speed bonus decreases with elapsed time, using the server's answer-receipt time. Scores never decrease.

## Technology and architecture

| Component | Implementation |
| --- | --- |
| Interface | React, TypeScript, and CSS |
| Application framework | Vinext with Vite |
| Backend | API route running on Cloudflare Workers |
| Shared room state | Cloudflare D1 |
| Schema and migrations | Drizzle |
| Live updates | Client polling approximately every 900 milliseconds |
| Hosting | Sites with an HTTPS deployment |

The server owns room state, answer acceptance, deadlines, and scoring. Version-checked database updates help prevent simultaneous player actions from overwriting one another. Correct answers are withheld from the client during the active question.

This version uses polling rather than WebSockets. Live updates can therefore take roughly one polling interval, plus network latency, to reach another device.

## My contribution and use of AI

I shaped the project concept, selected trivia as the game type, defined the intended group size, reviewed and approved the gameplay and scoring rules, and directed the progression from a local core-loop prototype to a publicly accessible HTTPS deployment.

AI generated the implementation and assisted with testing and deployment. This is an AI-assisted learning project; it is not presented as independently hand-coded work.

## Development process

1. Discussed the game concept before implementation.
2. Reviewed and approved rules, timing, scoring, and win conditions.
3. Built the room and multiplayer game loop.
4. Tested the local preview.
5. Published the game over HTTPS and enabled public access.

## Validation completed

During development, AI-assisted browser testing exercised a full game across four separate player tabs:

- Room creation and joining.
- Readiness checks and host-controlled start.
- Shared questions and countdown.
- Locked answers and correct/incorrect scoring.
- Speed bonuses and leaderboard updates.
- All ten rounds, final standings, and replay back to the lobby.

TypeScript checking and the production build also passed. These checks establish a working prototype; they are not a load test, formal security audit, or verification across all mobile browsers.

## Challenges and lessons

- **Shared timing:** a server-owned deadline gives every player the same answer window.
- **Concurrent actions:** room updates need conflict handling when several players answer together.
- **Fair scoring:** answer receipt and scoring belong on the server.
- **State consistency:** clients should avoid replacing a newer room state with a delayed older response.
- **Scope:** validating the core game loop before adding polish makes failures easier to identify.

## Current scope and next steps

The initial question bank contains 30 questions, with ten selected per game. A replay avoids the immediately preceding set. Rooms expire after 24 hours.

Planned improvements:

- Expand and review the question bank.
- Add category and difficulty selection.
- Improve reconnect handling and inactive-player management.
- Show the selected answer more clearly while waiting for the reveal.
- Add sound controls and winner animations.
- Add rate limiting, abuse protection, and broader concurrency testing.
- Test on more physical devices and browsers.
- Evaluate WebSockets or another push-based approach for faster updates.

## Repository contents

This folder documents the deployed project, its rules, architecture, development process, and roadmap. Application source code is not included in this folder.
