# Prediction Party 

A webxdc app for Delta Chat. Set up a prediction league
for your group chat that works for any episodic show or one-off event.

Members predict outcomes before each question locks, the host sets the results,
and a live leaderboard tracks who's winning. 

# Features

### One season per app

Each app instance is one season of one show / one event. To run a new season
or a different show, send a fresh copy of the app into the chat. This keeps each
league's history, contestants, and scores cleanly separated.

### Series vs. event

When you start a season the app asks what you're predicting:

- **📺 Recurring series** — episodes/rounds over time. Questions are grouped by **round/episode**, and the roster
  tracks contestants you can mark **out** as they're eliminated.
- **🎟️ One-off event** — a single night of categories (the Oscars, a finale, a
  game). Questions are a flat list; the roster is just a shared name pool.

### Question types

- **Pick one** — "Who's voted out?", "Best Picture", "Which team is eliminated?"
- **Pick several** — choose any number of options, scored with **conviction**: each correct
  pick scores the question's points, and every option you select beyond the first costs
  1 point (floored at 0). So `score = max(0, points × correct − (picks − 1))`. Committing to
  one bold pick beats spraying, while still rewarding finding every correct answer when a
  question worth ≥2 points has several. A hard pick cap is optional. (e.g. "who wins
  immunity?" to hedge, or "pick the 2 who make the merge" to identify a set.)
- **Rank in order** — players order the options (e.g. "predict the final 3, in order",
  "finishing order of the remaining teams"). The host can limit how many slots to rank
  (e.g. top 3 from a larger pool) and choose a scoring mode:
  - **Exact** — points for each slot placed in the same spot as the actual result.
  - **Proximity** — each item scores `max(0, points − places it's off)`, so near-misses
    still earn. Make the points at least one less than the number of places (5 places →
    4+ points) so closeness counts all the way down; only the worst-possible spot scores 0.
    Near-but-not-exact placements show in amber on the results.
- **Yes / No** 

### Live presence & notifications

- **Presence** — the header shows stacked avatars of everyone who has the app
  open right now
- **Leader notifications** — when a round/episode is fully scored (or, for a
  one-off event, when every question is scored), the app fires a webxdc
  notification to the group announcing the current leader (e.g. "🏆 Episode 1
  all scored — Alice leads with 7 pts").

# How it works

1. Anyone opens the app, picks series/event, names the season, and taps
   Start. They become the host.
2. The host uses **Manage** to add questions (and optionally a roster of
   contestants/nominees that fills question options with one tap), and to add
   co-hosts.
3. Everyone predicts on the **Predict** tab and can change picks until the
   host locks a question. Picks and tallies are shown openly — this app is
   built for high-trust groups.
4. The host sets the result; points are awarded automatically and the
   **Leaderboard** updates for everyone.
5. For a recurring series, the host taps **Finish season** when it's over to
   freeze picks and crown the champion (reopenable if needed).

The reducer enforces fairness deterministically: a prediction only counts if it
was made before the host locked the question, and resolving freezes it.
