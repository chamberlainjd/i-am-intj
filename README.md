<p align="center">
  <img src="./logo.svg" alt="i-am-intj" width="140" />
</p>
<p align="center">
  <strong align="center">The verdict comes first. The pleasantries don't come at all.</strong>
</p>
<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/github/license/chamberlainjd/i-am-intj?style=flat" alt="License"></a>
</p>


## Install

Claude Code only, by design.

```bash
claude plugin marketplace add chamberlainjd/i-am-intj
claude plugin install i-am-intj@i-am-intj
```

Then type `/i-am-intj`. No local clone needed: Claude Code fetches the repo and keeps it updated.

Want it on every session? `touch ~/.claude/.i-am-intj-always` (see [INSTALL.md](./INSTALL.md)).

No `claude` CLI on the machine? Copy the skill folder instead:

```bash
git clone https://github.com/chamberlainjd/i-am-intj
cp -R i-am-intj/skills/i-am-intj ~/.claude/skills/
```

The Codex, Cursor, Gemini, Zed, and Antigravity adapters that shipped with the upstream project were removed on purpose. One harness, maintained properly, beats seven maintained badly.

## What it does

A skill that makes Claude answer like an INTJ: verdict in the first line, one firm recommendation instead of an options buffet, reasoning compressed to what actually carries the conclusion, wrong premises challenged before any work starts, and no "Great question!" anywhere in the transcript.

## What changes

<table>
<tr>
<td width="50%">

## Before

> Great question! There are several excellent options for your database, and the right choice really depends on your specific needs. PostgreSQL is a fantastic, battle-tested relational database with a rich ecosystem. MongoDB, on the other hand, offers flexible schemas and can be a great fit for evolving data models. Both are solid choices! Would you like me to go deeper on either one? Happy to help you weigh the trade-offs!

</td>

<td width="50%">

## After

> Postgres. Your data is relational (tenants, invoices, line items) and you need the transactions; Mongo trades those away for schema flexibility you don't need.
>
> Mongo wins only if you expect document-shaped, per-customer schemas. You don't.
>
> Next: `createdb invoicing && npx prisma init`.

</td>
</tr>
</table>


## The rules

10 rules. Full text in [SKILL.md](./skills/i-am-intj/SKILL.md).

1. Verdict first.
2. One recommendation, not a menu.
3. Cut the social layer.
4. Challenge wrong premises before executing.
5. Show only the load-bearing logic.
6. Precision or silence.
7. Name the second-order effects.
8. Assume intelligence.
9. Do not ask what you can infer.
10. Controlled tone, rationed humor.

## Tune it

Fork, edit `skills/i-am-intj/SKILL.md`, then swap your copy in:

```bash
claude plugin uninstall i-am-intj            # drop this copy first:
claude plugin marketplace remove i-am-intj   # fork and origin share both names
claude plugin marketplace add <your-username>/i-am-intj
claude plugin install i-am-intj@i-am-intj
```

Restart Claude Code, then re-invoke `/i-am-intj`.

## Credits

Forked from [ayghri/i-have-adhd](https://github.com/ayghri/i-have-adhd): same machinery, different reader. The ruleset draws on 16personalities' Architect profile and Personality Junkie's INTJ communication write-ups. MBTI is a preference framework, not settled science; this repo takes no position on that, only on whether the output style is useful.

## License

MIT.

Star it if the reasoning holds. If it doesn't, open an issue and say exactly why.
