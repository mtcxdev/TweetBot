# TweetBot

TweetBot is a small Python command-line scheduler that publishes prepared posts to X at a configured interval. It uses Tweepy to authenticate with the X API and the Schedule library to run the posting task.

## Features

- Reads post text from `tweets.txt`, one post per line.
- Publishes the next eligible post at the configured interval.
- Logs successful posts with a local timestamp.
- Avoids repeating the same post during the current process session.

## Requirements

- Python 3.9 or later
- An X Developer account and API access that permits creating posts
- API credentials for the account that will publish
- The Python packages `tweepy` and `schedule`

Install the dependencies:

```bash
python -m pip install tweepy schedule
```

## Configuration

Open `config.py` and replace the placeholder values with your own credentials. Keep this file private and never commit real tokens or keys.

```python
BEARER_TOKEN = "YOUR_BEARER_TOKEN"
API_KEY = "YOUR_API_KEY"
API_SECRET = "YOUR_API_SECRET"
ACCESS_TOKEN = "YOUR_ACCESS_TOKEN"
ACCESS_TOKEN_SECRET = "YOUR_ACCESS_TOKEN_SECRET"
INTERVAL = 60  # Minutes between scheduled posts
```

Set `INTERVAL` to the number of minutes to wait between scheduled posts.

## Prepare posts

Add one post per line in `tweets.txt`. Keep each post within the current character and content limits enforced by X. The scheduler reads this file when it runs the posting task.

## Run

From the repository directory, start the scheduler with:

```bash
python farm.py
```

The scheduler runs until the process is stopped. It waits for the configured interval before the first scheduled post, then checks for pending work once per second.

## Important behavior and limitations

- The in-memory duplicate set resets when the program restarts. It is not a persistent posting history.
- The current implementation does not remove posts from `tweets.txt`; it filters posts already sent during the current session.
- If there are no eligible posts, the scheduler prints a message and waits for the next interval.
- API access, rate limits, and account restrictions are controlled by X. Review the applicable platform rules before automating posts.
- For long-running use, run the process in a managed environment and monitor its logs.

## Project files

| File | Purpose |
| --- | --- |
| `farm.py` | Initializes Tweepy and runs the scheduled posting loop |
| `config.py` | API credentials and posting interval |
| `tweets.txt` | Queue of post text, one post per line |
| `verify.py` | Supporting credential or API verification utility |
| `start.sh` | Shell entry point |

## License

See [LICENSE](LICENSE) for license terms.
