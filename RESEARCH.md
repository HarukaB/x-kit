# Project Usage Research

## Setup
- Install dependencies with `bun install` (requires network access to npm mirrors).
- Configure an `.env` file with:
  ```
  AUTH_TOKEN=your_x_auth_token
  GET_ID_X_TOKEN=token_for_getting_user_id
  ```
- Add user information in `dev-accounts.json`. Example:
  ```json
  {
    "username": "example",
    "twitter_url": "https://x.com/example",
    "description": "description",
    "tags": ["tag1", "tag2"]
  }
  ```
- Run scripts with Bun:
  ```bash
  bun run scripts/index.ts          # Fetch and save user profiles
  bun run scripts/fetch-tweets.ts   # Fetch home timeline tweets
  bun run scripts/fetch-user-tweets.ts # Example of fetching tweets of a specific user
  bun run scripts/batch-follow.ts   # Follow all saved accounts
  ```
- User data is stored under `accounts/` and tweets under `tweets/` (daily JSON files).

## Backup All Tweets/Comments/Media Idea
1. **Create authenticated client** using `XAuthClient()` from `scripts/utils.ts`.
2. **Fetch user tweets** with `client.getTweetApi().getUserTweets({ userId })`.
   - Handle pagination via the `cursor` field from API responses to loop until all pages are retrieved.
3. **Extract media** from each tweet's `extendedEntities.media` field (images and videos) as in `fetch-tweets.ts`.
4. **Fetch replies/comments** for each tweet using the tweet detail endpoint (e.g., `getTweetApi().getTweetDetail`). Combine replies recursively if needed.
5. **Save results** into JSON files under `tweets/<username>/` for organization. Optionally download media URLs to local storage.
6. **Support multiple users** by looping over user IDs defined in `dev-accounts.json` and repeating the process.

This approach leverages the existing utilities and the `twitter-openapi-typescript` client to programmatically backup a user's entire posting history along with associated media and discussions.
