# telegram_notify_action


## Usage

Send custom message

```yml
name: Check new upstream commits

on:
  push:
  repository_dispatch:
    types: [check_upstream]

jobs:
  check_upstream_commits:
    runs-on: ubuntu-latest
    name: Check upstream latest commits

    steps:
    - name: Checkout main
      uses: actions/checkout@v2

    - name: Fetch upstream changes
      id: sync
      uses: ivanmilov/upstream_check_new_commits@v1
      with:
        upstream_repository: i3/go-i3
        upstream_branch: master
        target_branch: master_upstream

    - name: Notify if new commits
      if: ${{ steps.sync.outputs.has_new_commits == 'true' }}
      uses: ivanmilov/telegram_notify_action@v1
      with:
        api_key: ${{secrets.TELEGRAM_API_KEY}}
        chat_id: ${{secrets.TELEGRAM_CHAT_ID}}
        message: "New commit in upstream repo ${{github.repository}}"
```

## Inputs

[Telegram Bot API](https://core.telegram.org/bots/api)

* `api_key`: Telegram authorization token
* `chat_id`: unique identifier for this chat
* `message`: optional. custom message. (default "Hey")
