# LovelyAuth skill

An agent skill for installing the [LovelyAuth CLI](https://www.npmjs.com/package/lovelyauth), connecting your vault, and retrieving verification codes during sign-in.

Tell your agent:

> Please install the skill from https://github.com/LovelyAuth/lovelyauth-skill and set up LovelyAuth.

The agent should install the skill using its supported skill installer, install or update to the latest CLI from npm, and start device login. It will give you an approval link and code. Open the link, sign in, and approve the connection; the agent then verifies access to your vault.

After setup, ask:

> Use LovelyAuth to get the verification code for my GitHub account.

Or, as part of a larger task:

> Sign in to GitHub and use LovelyAuth when it asks for my verification code.

## Installation details

The skill entry point is [`SKILL.md`](SKILL.md) at the repository root. For agents that require manual skill installation, copy this repository's contents into a skill directory named `lovelyauth` in that agent's supported skills location. Follow the agent's instructions for loading newly installed skills.

The CLI requires **Node.js 24+**, npm, and an internet connection. The skill guides the agent through:

```sh
npm install -g lovelyauth@latest
lovelyauth login device
lovelyauth list
lovelyauth get <slug>
```

Before every login, the agent checks `npm view lovelyauth version` and updates the CLI if a newer version is published. You do not need an npm account or a device code in advance. The login command generates the approval link and code. Once connected, the CLI remembers the session for later requests.

Only connect a vault on a machine and with an agent you trust to handle its verification codes. The skill does not grant permission to change or delete your vault accounts.

[LovelyAuth](https://lovelyauth.com/) · [Web vault](https://app.lovelyauth.com/) · [CLI guide](https://lovelyauth.com/cli)
