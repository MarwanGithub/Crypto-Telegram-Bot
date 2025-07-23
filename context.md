# Project Summary: Cryptohopper to Telegram Bot

This project is a Python script that automatically fetches trading data from a Cryptohopper account and posts formatted signals to a private Telegram channel.

## Current Status
- The script is fully functional on the local machine.
- It is deployed and running on GitHub Actions, scheduled to run every 5 minutes.
- The script correctly fetches the latest trades from the Cryptohopper `/trade` API endpoint.
- It intelligently processes only new trades by saving the last seen trade ID to a `last_trade_id.txt` file, which is committed back to the repository by the GitHub Action.
- It formats buy/sell signals into clean messages for Telegram, including a randomly generated "accuracy" score for buy signals.
- It filters trades to only send signals for automated strategies (e.g., `strategy`, `trailing-stop-loss`), ignoring manual trades.
- All secrets and credentials (API tokens, IDs) have been removed from the code and are securely managed using GitHub Actions Secrets.

## Key Decisions & Technical Details
- **Hosting:** We are using **GitHub Actions** for automation because its free tier allows for a 5-minute execution interval, which is superior to Render's 15-minute minimum.
- **Bot "Memory":** The bot's state (the last trade ID) is persisted by having the GitHub Actions workflow commit the `last_trade_id.txt` file back to the repository after each run. This required granting `contents: write` permissions to the workflow.
- **API Endpoint:** After investigation, we determined that the Cryptohopper `/trade` endpoint is the only reliable source for bot activity. The `/signal` endpoint is for external signalers, and the `/airesults` endpoint was found to be undocumented and returned a server error.
- **Dependencies:** The only external Python library required is `requests`.

## Next Steps / Future Improvements
- Monitor the GitHub Actions logs to ensure the bot continues to run smoothly.
- Potentially optimize the workflow run time by caching Python dependencies to stay well within the GitHub free tier limits.
- The user may wish to further customize the formatting of the Telegram messages.