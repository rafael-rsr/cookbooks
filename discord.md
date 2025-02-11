# Discord

<!--
https://gist.github.com/thomasbnt/b6f455e2c7d743b796917fa3c205f812
-->

<!--
<a href="https://discord.gg/zxHM7uwDmk">
  <img src="https://img.shields.io/discord/873418656405651487.svg?label=&logo=discord&logoColor=ffffff&color=7389D8&labelColor=6A7EC2" alt="Discord">
</a>
-->

## Links

- [Status Page](https://discordstatus.com/)

## Tools

- [Discord Webhook Tester](https://lew.la/webhook/)

## App

### Installation

#### Homebrew

```sh
brew install --cask discord
```

#### Chocolatey

```sh
choco install -y discord
```

## API

### Tips

```sh
export MSG_TITLE='Knock Knock!'
export MSG_LINK='https://github.com/github'
export MSG_COLOR='0x2ea043'
export MSG_VALUE="Who's There?"

export PAYLOAD="$(jq \
  --null-input \
  --compact-output \
  --monochrome-output \
  --arg title "$MSG_TITLE" \
  --arg link "$MSG_LINK" \
  --arg color "$MSG_COLOR" \
  --arg message "$MSG_VALUE" \
  '{
    username: "GitHub",
    avatar_url: "https://avatars.githubusercontent.com/u/9919",
    embeds: [{
      title: $title,
      url: $link,
      color: $color,
      description: $message
    }]
  }')"; echo $PAYLOAD

export DISCORD_CHANNEL=''
export DISCORD_TOKEN=''

curl \
  -fSs \
  -H 'Content-Type: application/json' \
  -d "$PAYLOAD" \
  "https://discord.com/api/webhooks/${DISCORD_CHANNEL}/${DISCORD_TOKEN}"
```
