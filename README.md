# WYK Online Judge
[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![Code style: yapf](https://img.shields.io/badge/code%20style-yapf-blue)](https://github.com/google/yapf)
[
    ![Imports: isort](https://img.shields.io/badge/%20imports-isort-%231674b1?style=flat&labelColor=ef8336)
](https://pycqa.github.io/isort/)

An online judge with tasks and contests.
<br>
Judge backend: [wykoj/wykoj-judge](https://github.com/wykoj/wykoj-judge)

Live Version: https://wykoj.owo.idv.hk

UI based on [HKOI Online Judge](https://judge.hkoi.org).

## Installation
- Clone repo with `git clone https://github.com/jonowo/wykoj`.
- Compile (and minify) `wykoj/scss/style.scss` to `wykoj/static/style.min.css`
  (Settings are configured for the VS Code
  [Live SASS Compiler](https://marketplace.visualstudio.com/items?itemName=ritwickdey.live-sass) extension.)
- Install/Upgrade dependencies: `pip install -U -r requirements.txt`.
- Create `config.json` in the inner `wykoj` directory with the following keys:
  - `JUDGE_HOST` - Domain of judging backend, e.g. `https://example.com` (without trailing slash).
  - `SECRET_KEY` - A URL-safe secret key, can be generated with `secrets.token_hex(16)`.
  - `DB_URI` - A database URI including login credentials.
- Initialize database: `python init_db.py`. (You will be asked to install the appropriate
  [database driver](https://tortoise-orm.readthedocs.io/en/latest/getting_started.html).)
  - An admin user with username `admin` and password `adminadmin` will be created.
    (Please change username and password upon first login.)
- Create a (private) GitHub repo to store test cases. It will be used as a submodule.
  - Remove `.gitmodules`
  - Run: `git submodule add [repo link] wykoj/test_cases`
  - Create a webhook for just the push event
    - Payload URL: `[your domain]/github_push`
    - Content type: `application/json`
    - Secret: `SECRET_KEY` from above
    - Events: `push` only
- Run: `hypercorn -b 0.0.0.0:3000 "wykoj:create_app()"`.

Access the online judge at http://localhost:3000.

## Issues
Multiple submisions from the same user to the same task are marked `first_solve=True`.
Drop `first_solve` and `solves` columns and compute first solve instead.
Or use locks when saving submissions instead.

## Roadmap
- Check judge online in the background instead of on user request
- Result auto refresh data during contest
- Recommended tasks (unsolved tasks ordered by solved descending)
- Add language specs to Info page
- Including previous subtask in subtask
- Create a proper frontend API instead of hiding data in invisible elements
- Batch user creation
- An option in settings to show [REDACTED] on the home page
- Chess rating leaderboard
- Lichess games
- Stats in user page and contests page
- Task stats page (hide link during contest, contest redirect)
- Replace refreshing of submission page in `main.js` with
  https://pgjones.gitlab.io/quart/tutorials/broadcast_tutorial.html
  or just make an api route to check if finished judging
- Spinning Ame animation on submission page if pending
- Custom page creation (admin)
- Advanced filtering form footer for submissions page
- Categorization for tasks
- Groups and assignments
