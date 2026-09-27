<div align="center">

# ✖ ｌｕａｒｏｔ－ｄｅｏｂ ✖

### `.:*~*:._ pulls the source out from behind the loadstring _.:*~*:.`

![python](https://img.shields.io/badge/python-3.10%2B-000000?style=flat-square)
![deps](https://img.shields.io/badge/deps-zero-ff0066?style=flat-square)
![license](https://img.shields.io/badge/license-MIT-000000?style=flat-square)
![tests](https://img.shields.io/badge/tests-13%20passing-ff0066?style=flat-square)

**[★ JOIN THE DISCORD FOR MORE STUFF LIKE THIS ★](https://discord.gg/9bgECTqeE)**

</div>

---

## `[ x ]` real talk first

luarot **isnt an obfuscator.** it dont scramble ur code. it *hosts* it.

u get a one-liner. the source never ships w/ it — it sits on their api, chopped
into hops, each one carrying an anti-sandbox guard that type-checks
`GetProductInfo` and tells u `"yh, fuck u skid"` if it thinks ur faking roblox.

so theres nothing to un-mangle. u just gotta **walk the chain and peel the
boilerplate.** thats what this does. ♥

```
$ python3 luarot.py 01c0fbf793658654903ea590

print("Hello From LUarottttttttttttttttt")
```

3 hops. 2329 bytes of guard. **43 bytes of actual script.**

## `[ x ]` setup

```bash
unzip luarot-deob.zip && cd luarot-deob
python3 luarot.py --offline samples/stage1.lua samples/stage2.lua samples/stage3.lua
```

python 3.10+. **zero dependencies.** stdlib only. nothing 2 install.

## `[ x ]` usage

```bash
python3 luarot.py "loadstring(game:HttpGet('https://luarot.vercel.app/api-load/ID'))()"
python3 luarot.py ID                      # bare hex id works
python3 luarot.py https://.../api-load/ID -o clean.lua
python3 luarot.py ID --report             # chain map + risk scan
python3 luarot.py ID --stages ./dump      # keep every raw hop
python3 luarot.py ID --keep-guards        # leave the boilerplate in
python3 luarot.py --offline a.lua b.lua   # no network at all
```

`--report` gives u the whole chain w/ sizes + hashes:

```
  stages fetched    : 3
      [0] 01c0fbf79365     792B  sha=bc1909344fe9  -> 4c1ac2310591
      [1] 4c1ac2310591     792B  sha=3d528e15bb9c  -> 43110ece1f19
      [2] 43110ece1f19     745B  sha=dea7dd96bd7b  -> terminal
  guards stripped   : 3
  chain hops removed: 2
  risk scan         : nothing flagged
```

## `[ x ]` it never runs the payload

**not once. not sandboxed. not ever.** every stage is treated as text.

loader chains r how half the stealers in this scene get shipped. a tool that
`loadstring`s what it downloads 2 "analyze" it is just a dropper w/ extra steps.
so this one parses. thats it.

## `[ x ]` it also reads it 4 u

once the source is out, it costs nothing 2 flag the lines worth a second look:

| flag | what it means |
|---|---|
| `discord-webhook` | **HIGH** — the standard exfil endpoint |
| `cookie-access` | **HIGH** — touching `.ROBLOSECURITY` |
| `remote-exec` | **HIGH** — loads even more remote code |
| `http-post` / `player-identity` | MED — outbound data + who u r |
| `executor-api` / `filesystem` | MED — `getgenv`, `writefile`, etc |

signals, not verdicts. plenty of clean scripts POST. but if u see webhook +
identity + cookie on one script, u already know. ✖

it also sniffs whether the payload underneath is protected by smthn **else** —
luraph, luast, ironbrew, moonsec — and tells u to go get the right tool.

## `[ x ]` it wont save u from

* **chains that branch.** it follows one `HttpGet` per hop. conditional loaders
  that pick a url at runtime need `--stages` + ur own eyes.
* **payloads behind auth / key systems.** if the api wants a header or a hwid u
  dont have, u get the http error, not the source.
* **the endpoint going down.** the source lives on their server, not in the
  one-liner. luarot deletes it, its gone. thats the whole product.
* **actual obfuscation underneath.** it tells u whats there, doesnt crack it.

## `[ x ]` proof

```bash
python3 tests/test_luarot.py        # 13 tests, offline
LUAROT_NET=1 python3 tests/test_luarot.py   # + live chain
```

---

<div align="center">

### `✖ ✖ ✖`

chain didnt resolve? run `--report --stages ./dump` and send both.

### **[★ discord.gg/9bgECTqeE ★](https://discord.gg/9bgECTqeE)**
#### `FOR MORE STUFF LIKE THIS!!`

MIT — see [LICENSE](LICENSE). read ur own scripts, check what ppl tell u 2 run,
analyze malware. respect what u point it at.

`xXx` *stay dangerous* `xXx`

</div>
