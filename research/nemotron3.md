
===== RUN 2026-08-07 09:09:44 UTC | model nemotron3 | target google =====
[0m
> build · nemotron-3-ultra-free
[0m
[0m→ [0mRead scope.yml
[0m% [0mWebFetch https://bughunters.google.com/about/rules
[0m
[0m$ [0mcurl -s "https://api.github.com/orgs/google/repos?per_page=100&sort=updated" | head -c 5000
[
  {
    "id": 58034548,
    "node_id": "MDEwOlJlcG9zaXRvcnk1ODAzNDU0OA==",
    "name": "flexbox-layout",
    "full_name": "google/flexbox-layout",
    "private": false,
    "owner": {
      "login": "google",
      "id": 1342004,
      "node_id": "MDEyOk9yZ2FuaXphdGlvbjEzNDIwMDQ=",
      "avatar_url": "https://avatars.githubusercontent.com/u/1342004?v=4",
      "gravatar_id": "",
      "url": "https://api.github.com/users/google",
      "html_url": "https://github.com/google",
      "followers_url": "https://api.github.com/users/google/followers",
      "following_url": "https://api.github.com/users/google/following{/other_user}",
      "gists_url": "https://api.github.com/users/google/gists{/gist_id}",
      "starred_url": "https://api.github.com/users/google/starred{/owner}{/repo}",
      "subscriptions_url": "https://api.github.com/users/google/subscriptions",
      "organizations_url": "https://api.github.com/users/google/orgs",
      "repos_url": "https://api.github.com/users/google/repos",
      "events_url": "https://api.github.com/users/google/events{/privacy}",
      "received_events_url": "https://api.github.com/users/google/received_events",
      "type": "Organization",
      "user_view_type": "public",
      "site_admin": false
    },
    "html_url": "https://github.com/google/flexbox-layout",
    "description": "Flexbox for Android ",
    "fork": false,
    "url": "https://api.github.com/repos/google/flexbox-layout",
    "forks_url": "https://api.github.com/repos/google/flexbox-layout/forks",
    "keys_url": "https://api.github.com/repos/google/flexbox-layout/keys{/key_id}",
    "collaborators_url": "https://api.github.com/repos/google/flexbox-layout/collaborators{/collaborator}",
    "teams_url": "https://api.github.com/repos/google/flexbox-layout/teams",
    "hooks_url": "https://api.github.com/repos/google/flexbox-layout/hooks",
    "issue_events_url": "https://api.github.com/repos/google/flexbox-layout/issues/events{/number}",
    "events_url": "https://api.github.com/repos/google/flexbox-layout/events",
    "assignees_url": "https://api.github.com/repos/google/flexbox-layout/assignees{/user}",
    "branches_url": "https://api.github.com/repos/google/flexbox-layout/branches{/branch}",
    "tags_url": "https://api.github.com/repos/google/flexbox-layout/tags",
    "blobs_url": "https://api.github.com/repos/google/flexbox-layout/git/blobs{/sha}",
    "git_tags_url": "https://api.github.com/repos/google/flexbox-layout/git/tags{/sha}",
    "git_refs_url": "https://api.github.com/repos/google/flexbox-layout/git/refs{/sha}",
    "trees_url": "https://api.github.com/repos/google/flexbox-layout/git/trees{/sha}",
    "statuses_url": "https://api.github.com/repos/google/flexbox-layout/statuses/{sha}",
    "languages_url": "https://api.github.com/repos/google/flexbox-layout/languages",
    "stargazers_url": "https://api.github.com/repos/google/flexbox-layout/stargazers",
    "contributors_url": "https://api.github.com/repos/google/flexbox-layout/contributors",
    "subscribers_url": "https://api.github.com/repos/google/flexbox-layout/subscribers",
    "subscription_url": "https://api.github.com/repos/google/flexbox-layout/subscription",
    "commits_url": "https://api.github.com/repos/google/flexbox-layout/commits{/sha}",
    "git_commits_url": "https://api.github.com/repos/google/flexbox-layout/git/commits{/sha}",
    "comments_url": "https://api.github.com/repos/google/flexbox-layout/comments{/number}",
    "issue_comment_url": "https://api.github.com/repos/google/flexbox-layout/issues/comments{/number}",
    "contents_url": "https://api.github.com/repos/google/flexbox-layout/contents/{+path}",
    "compare_url": "https://api.github.com/repos/google/flexbox-layout/compare/{base}...{head}",
    "merges_url": "https://api.github.com/repos/google/flexbox-layout/merges",
    "archive_url": "https://api.github.com/repos/google/flexbox-layout/{archive_format}{/ref}",
    "downloads_url": "https://api.github.com/repos/google/flexbox-layout/downloads",
    "issues_url": "https://api.github.com/repos/google/flexbox-layout/issues{/number}",
    "pulls_url": "https://api.github.com/repos/google/flexbox-layout/pulls{/number}",
    "milestones_url": "https://api.github.com/repos/google/flexbox-layout/milestones{/number}",
    "notifications_url": "https://api.github.com/repos/google/flexbox-layout/notifications{?since,all,participating}",
    "labels_url": "https://api.github.com/repos/google/flexbox-layout/labels{/name}",
    "releases_url": "https://api.github.com/repos/google/flexbox-layout/releases{/id}",
    "deployments_url": "https://api.github.com/repos/google/flexbox-layout/deployments",
    "created_at": "2016-05-04T08:11:22Z",
    "updated_at": "2026-08-07T09:01:56Z",
    "pushed_at": "2024-04-22T19:22:36Z",
    "git_url": "git://github.com/google/flexbox-layout.git",
    "ssh_url": "git@github.com:google/flexbox-layout.git",
    "clone_url": "https://github.com/google/flexbox-layout.git",
    "svn_
[0m
[0m$ [0mcurl -s "https://crt.sh/?q=%.google.com&output=json" | head -c 5000
(no output)

<shell_metadata>
shell tool terminated command after exceeding timeout 30000 ms. If this command is expected to take longer and is not waiting for interactive input, retry with a larger timeout value in milliseconds.
</shell_metadata>
[0m
[0m% [0mWebFetch https://bughunters.google.com/about/rules
[0m
[0m$ [0mcurl -s "https://crt.sh/?q=%.google.com&output=json" --max-time 60 | jq -r '.[].name_value' | sort -u | head -100
jq: parse error: Invalid numeric literal at line 1, column 7
[0m
[0m$ [0mcurl -s "https://api.github.com/orgs/google/repos?per_page=100&sort=updated" | jq -r '.[] | select(.fork==false) | "\(.name) \(.description // "") \(.html_url)"' | head -50
flexbox-layout Flexbox for Android  https://github.com/google/flexbox-layout
flatbuffers FlatBuffers: Memory Efficient Serialization Library https://github.com/google/flatbuffers
promises Promises is a modern framework that provides a synchronization construct for Swift and Objective-C. https://github.com/google/promises
guava Google core libraries for Java https://github.com/google/guava
sam SAM Sovereign Agent Mesh https://github.com/google/sam
skills Agent Skills for Google products and technologies https://github.com/google/skills
re2 RE2 is a fast, safe, thread-friendly alternative to backtracking regular expression engines like those used in PCRE, Perl, and Python. It is a C++ library. https://github.com/google/re2
cadvisor Analyzes resource usage and performance characteristics of running containers. https://github.com/google/cadvisor
agents-cli The CLI and skills that turn any coding assistant into an expert at creating, evaluating, and deploying AI agents on Google Cloud. https://github.com/google/agents-cli
zx A tool for writing better scripts https://github.com/google/zx
material-design-icons Material Design icons by Google (Material Symbols) https://github.com/google/material-design-icons
perfetto Production-grade client-side tracing, profiling, and analysis for complex software systems. https://github.com/google/perfetto
adk-kotlin  https://github.com/google/adk-kotlin
osv-scanner Vulnerability scanner written in Go which uses the data provided by https://osv.dev https://github.com/google/osv-scanner
recaptcha PHP client library for reCAPTCHA, a free service to protect your website from spam and abuse. https://github.com/google/recaptcha
wasefire Secure firmware framework focusing on developer experience https://github.com/google/wasefire
comprehensive-rust This is the Rust course used by the Android team at Google. It provides you the material to quickly teach Rust. https://github.com/google/comprehensive-rust
snappy A fast compressor/decompressor https://github.com/google/snappy
j2cl Java to Closure JavaScript transpiler https://github.com/google/j2cl
device-infra  https://github.com/google/device-infra
adk-go An open-source, code-first Go toolkit for building, evaluating, and deploying sophisticated AI agents with flexibility and control. https://github.com/google/adk-go
dawn Native WebGPU implementation. Mirror of https://dawn.googlesource.com/dawn. File bugs here: https://crbug.com/dawn/new https://github.com/google/dawn
android-cuttlefish Cuttlefish host support debian package. https://github.com/google/android-cuttlefish
GNM An open ecosystem of parametric human models and perception stacks, starting with GNM Head. https://github.com/google/GNM
or-tools Google's Operations Research tools: https://github.com/google/or-tools
security-research This project hosts security advisories and their accompanying proof-of-concepts related to research conducted at Google which impact non-Google owned code. https://github.com/google/security-research
tf-quant-finance High-performance TensorFlow library for quantitative finance. https://github.com/google/tf-quant-finance
latexify_py A library to generate LaTeX expression from Python code. https://github.com/google/latexify_py
dopamine Dopamine is a research framework for fast prototyping of reinforcement learning algorithms.  https://github.com/google/dopamine
langextract A Python library for extracting structured information from unstructured text using LLMs with precise source grounding and interactive visualization. https://github.com/google/langextract
diff-match-patch Diff Match Patch is a high-performance library in multiple languages that manipulates plain text. https://github.com/google/diff-match-patch
sentencepiece Unsupervised text tokenizer for Neural Network-based text generation. https://github.com/google/sentencepiece
leveldb LevelDB is a fast key-value storage library written at Google that provides an ordered mapping from string keys to string values. https://github.com/google/leveldb
grumpy Grumpy is a Python to Go source code transcompiler and runtime. https://github.com/google/grumpy
XNNPACK High-efficiency floating-point neural network inference operators for mobile, server, and Web https://github.com/google/XNNPACK
open-location-code Open Location Code is a library to generate short codes, called "plus codes", that can be used as digital addresses where street addresses don't exist. https://github.com/google/open-location-code
GoogleSignIn-iOS Enables iOS and macOS apps to sign in with Google. https://github.com/google/GoogleSignIn-iOS
closure-library Google's common JavaScript library https://github.com/google/closure-library
adk-java An open-source, code-first Java toolkit for building, evaluating, and deploying sophisticated AI agents with flexibility and control. https://github.com/google/adk-java
adk-samples A collection of sample agents built with Agent Development Kit (ADK)  https://github.com/google/adk-samples
adk-python An open-source, code-first Python toolkit for building, evaluating, and deploying sophisticated AI agents with flexibility and control. https://github.com/google/adk-python
web-game-samples  https://github.com/google/web-game-samples
crubit A bidirectional bindings generator for C++ and Rust. https://github.com/google/crubit
atheris  https://github.com/google/atheris
hdmitsuba USD Hydra delegate for Mitsuba 3 https://github.com/google/hdmitsuba
earthengine-catalog Earth Engine Public Data Catalog in STAC form https://github.com/google/earthengine-catalog
draco Draco is a library for compressing and decompressing 3D geometric meshes and point clouds. It is intended to improve the storage and transmission of 3D graphics. https://github.com/google/draco
osv.dev Open source vulnerability DB and triage service. https://github.com/google/osv.dev
sec-gemini Sec-Gemini  is a cutting-edge AI model designed to enhance cybersecurity capabilities and empower defenders in the ongoing battle against cyber threats. https://github.com/google/sec-gemini
tunix A Lightweight LLM Post-Training Library https://github.com/google/tunix
[0m
[0m$ [0mcurl -s "https://bughunters.google.com/about/rules" -H "Accept: text/html" | head -c 10000
<a href="https://bughunters.google.com/about/rules/about-this-section">Moved Permanently</a>.
[0m
[0m$ [0mcurl -s "https://crt.sh/?q=%.google.com&output=json" --max-time 120 | python3 -c "import sys, json; data=json.load(sys.stdin); [print(x.get('name_value','')) for x in data if x.get('name_value')]" | sort -u | head -200
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "/usr/lib/python3.12/json/__init__.py", line 293, in load
    return loads(fp.read(),
           ^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.12/json/__init__.py", line 346, in loads
    return _default_decoder.decode(s)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.12/json/decoder.py", line 337, in decode
    obj, end = self.raw_decode(s, idx=_w(s, 0).end())
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.12/json/decoder.py", line 355, in raw_decode
    raise JSONDecodeError("Expecting value", s, err.value) from None
json.decoder.JSONDecodeError: Expecting value: line 1 column 1 (char 0)
[0m
[0m$ [0mcurl -s "https://bughunters.google.com/about/rules" -L -H "Accept: text/html" | head -c 15000
<!doctype html>
<html lang="en-US">
  <head>
    <meta charset="utf-8" />
    <title>Rules | Google Bug Hunters</title>
    <meta property="twitter:card" content="summary" />
    <meta property="twitter:site" content="GoogleVRP" />
    <meta property="og:title" content="Rules | Google Bug Hunters" />
	<meta name="description" content="Welcome to Google&#39;s Bug Hunting community, learn more about hunting &amp; reporting bugs you’ve found in Google products." />
	<meta property="og:description" content="Welcome to Google&#39;s Bug Hunting community, learn more about hunting &amp; reporting bugs you’ve found in Google products." />
	<meta property="og:image" content="https://storage.googleapis.com/bughunters-social/og_social_image_bughunters.png" />
	<meta property="og:url" content="https://bughunters.google.com/about/rules/about-this-section" />
	<meta property="twitter:title" content="Rules | Google Bug Hunters" />
	<meta property="twitter:description" content="Welcome to Google&#39;s Bug Hunting community, learn more about hunting &amp; reporting bugs you’ve found in Google products." />
	<meta property="twitter:image" content="https://storage.googleapis.com/bughunters-social/og_social_image_bughunters.png" />
    <base href="/" />

    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <link
      rel="icon"
      type="image/png"
      href="https://bughunters.google.com/static/icon-192.png"
      sizes="192x192"
    />
    <link
      rel="apple-touch-icon"
      type="image/png"
      href="https://bughunters.google.com/static/touch-icon-180.png"
      sizes="180x180"
    />
    <link rel="alternate" type="application/rss+xml" title="Security Engineering Blog" href="/feed/en" />

    
    <link
      href="https://fonts.googleapis.com/css2?family=Google+Sans:wght@400;500&family=Roboto+Mono:wght@400&family=Material+Icons&family=Material+Icons+Outlined&display=swap"
      rel="stylesheet"
    />

    
    <link
      href="https://www.gstatic.com/glue/cookienotificationbar/cookienotificationbar.min.css"
      rel="stylesheet"
    />
    <link rel="stylesheet" href="https://www.gstatic.com/bughunters/960149923/static/css/third_party_css-bundle.css" />

    
    <link rel="stylesheet" href="https://www.gstatic.com/bughunters/960149923/static/css/styles.css" />

    
    <script id="gtm-init" nonce="968hfWv9agF6BT/v5gug4VsRcSI=">
      window.dataLayer = window.dataLayer || [];
      function gtag() {
        dataLayer.push(arguments);
      }

      // The cookie consent banner library invokes this callback after it has fully loaded and set the
      // default consent state in dataLayer of `denied` and updated based on local storage settings . This timing prevents GTM from firing tags before consent `defaults`
      // of denied are in place.
      function glueCookieNotificationBarLoaded() {
        var gtmNonce = document.getElementById("gtm-init").nonce;
        var script = document.createElement("script");
        script.async = true;
        script.src = "https://www.googletagmanager.com/gtag/js";
        script.nonce = gtmNonce;
        document.head.appendChild(script);
      }
    </script>
  </head>
  <body class="mat-app-background">
    <app-root></app-root>
    <script
      nonce="968hfWv9agF6BT/v5gug4VsRcSI="
      defer
      src="https://www.gstatic.com/bughunters/960149923/static/js/third_party_js-bundle.js"
    ></script>
    <script nonce="968hfWv9agF6BT/v5gug4VsRcSI=" defer src="https://www.gstatic.com/bughunters/960149923/app_bundle_prod.js"></script>
  </body>
</html>
[0m
[0m$ [0mcurl -s "https://crt.sh/?q=%.google.com&output=json" --max-time 120 | head -c 2000
<html>
<head><title>502 Bad Gateway</title></head>
<body>
<center><h1>502 Bad Gateway</h1></center>
<hr><center>nginx</center>
</body>
</html>
[0m
[0m$ [0mcurl -s "https://crt.sh/?q=google.com&output=json" --max-time 60 | head -c 5000
<html>
<head><title>502 Bad Gateway</title></head>
<body>
<center><h1>502 Bad Gateway</h1></center>
<hr><center>nginx</center>
</body>
</html>
[0m
[0m$ [0mcurl -s "https://api.github.com/repos/google/security-research/contents" | jq -r '.[] | "\(.name) \(.type) \(.html_url)"'
.github dir https://github.com/google/security-research/tree/master/.github
.gitignore file https://github.com/google/security-research/blob/master/.gitignore
.gitmodules file https://github.com/google/security-research/blob/master/.gitmodules
CONTRIBUTING.md file https://github.com/google/security-research/blob/master/CONTRIBUTING.md
LICENSE file https://github.com/google/security-research/blob/master/LICENSE
README.md file https://github.com/google/security-research/blob/master/README.md
SECURITY.md file https://github.com/google/security-research/blob/master/SECURITY.md
_config.yml file https://github.com/google/security-research/blob/master/_config.yml
analysis dir https://github.com/google/security-research/tree/master/analysis
kernelctf dir https://github.com/google/security-research/tree/master/kernelctf
kvmctf dir https://github.com/google/security-research/tree/master/kvmctf
pocs dir https://github.com/google/security-research/tree/master/pocs
v8ctf dir https://github.com/google/security-research/tree/master/v8ctf
[0m
[0m$ [0mcurl -s "https://api.github.com/repos/google/osv.dev/contents" | jq -r '.[] | "\(.name) \(.type) \(.html_url)"'
.agents dir https://github.com/google/osv.dev/tree/master/.agents
.gcloudignore file https://github.com/google/osv.dev/blob/master/.gcloudignore
.gemini dir https://github.com/google/osv.dev/tree/master/.gemini
.github dir https://github.com/google/osv.dev/tree/master/.github
.gitignore file https://github.com/google/osv.dev/blob/master/.gitignore
.gitmodules file https://github.com/google/osv.dev/blob/master/.gitmodules
.markdownlint.json file https://github.com/google/osv.dev/blob/master/.markdownlint.json
.pylintrc file https://github.com/google/osv.dev/blob/master/.pylintrc
.ruff.toml file https://github.com/google/osv.dev/blob/master/.ruff.toml
.style.yapf file https://github.com/google/osv.dev/blob/master/.style.yapf
AGENTS.md file https://github.com/google/osv.dev/blob/master/AGENTS.md
CHANGELOG.md file https://github.com/google/osv.dev/blob/master/CHANGELOG.md
CLAUDE.md file https://github.com/google/osv.dev/blob/master/CLAUDE.md
CONTRIBUTING.md file https://github.com/google/osv.dev/blob/master/CONTRIBUTING.md
LICENSE file https://github.com/google/osv.dev/blob/master/LICENSE
Makefile file https://github.com/google/osv.dev/blob/master/Makefile
README.md file https://github.com/google/osv.dev/blob/master/README.md
actions dir https://github.com/google/osv.dev/tree/master/actions
bindings dir https://github.com/google/osv.dev/tree/master/bindings
cloudbuild.yaml file https://github.com/google/osv.dev/blob/master/cloudbuild.yaml
deployment dir https://github.com/google/osv.dev/tree/master/deployment
docker dir https://github.com/google/osv.dev/tree/master/docker
docs dir https://github.com/google/osv.dev/tree/master/docs
external dir https://github.com/google/osv.dev/tree/master/external
gcp dir https://github.com/google/osv.dev/tree/master/gcp
go dir https://github.com/google/osv.dev/tree/master/go
mlc_config.json file https://github.com/google/osv.dev/blob/master/mlc_config.json
osv-scanner.toml file https://github.com/google/osv.dev/blob/master/osv-scanner.toml
osv.dev.code-workspace file https://github.com/google/osv.dev/blob/master/osv.dev.code-workspace
osv dir https://github.com/google/osv.dev/tree/master/osv
poetry.lock file https://github.com/google/osv.dev/blob/master/poetry.lock
pyproject.toml file https://github.com/google/osv.dev/blob/master/pyproject.toml
renovate.json file https://github.com/google/osv.dev/blob/master/renovate.json
run_tests.sh file https://github.com/google/osv.dev/blob/master/run_tests.sh
source.yaml file https://github.com/google/osv.dev/blob/master/source.yaml
source_test.yaml file https://github.com/google/osv.dev/blob/master/source_test.yaml
tools dir https://github.com/google/osv.dev/tree/master/tools
vulnfeeds dir https://github.com/google/osv.dev/tree/master/vulnfeeds
website symlink https://github.com/google/osv.dev/blob/master/website
[0m
[0m$ [0mcurl -s "https://api.github.com/repos/google/security-research/contents/pocs" | jq -r '.[] | "\(.name) \(.type) \(.html_url)"'
LibRaw dir https://github.com/google/security-research/tree/master/pocs/LibRaw
apple dir https://github.com/google/security-research/tree/master/pocs/apple
azure dir https://github.com/google/security-research/tree/master/pocs/azure
bios dir https://github.com/google/security-research/tree/master/pocs/bios
bleve dir https://github.com/google/security-research/tree/master/pocs/bleve
cpus dir https://github.com/google/security-research/tree/master/pocs/cpus
grub dir https://github.com/google/security-research/tree/master/pocs/grub
kakadu dir https://github.com/google/security-research/tree/master/pocs/kakadu
linux dir https://github.com/google/security-research/tree/master/pocs/linux
log4j dir https://github.com/google/security-research/tree/master/pocs/log4j
oracle dir https://github.com/google/security-research/tree/master/pocs/oracle
[0m
[0m$ [0mcurl -s "https://api.github.com/repos/google/osv.dev/contents/go" | jq -r '.[] | "\(.name) \(.type) \(.html_url)"'
.golangci.yaml file https://github.com/google/osv.dev/blob/master/go/.golangci.yaml
Dockerfile file https://github.com/google/osv.dev/blob/master/go/Dockerfile
cloudbuild.yaml file https://github.com/google/osv.dev/blob/master/go/cloudbuild.yaml
cmd dir https://github.com/google/osv.dev/tree/master/go/cmd
go.mod file https://github.com/google/osv.dev/blob/master/go/go.mod
go.sum file https://github.com/google/osv.dev/blob/master/go/go.sum
internal dir https://github.com/google/osv.dev/tree/master/go/internal
logger dir https://github.com/google/osv.dev/tree/master/go/logger
osv dir https://github.com/google/osv.dev/tree/master/go/osv
purl dir https://github.com/google/osv.dev/tree/master/go/purl
run_tests.sh file https://github.com/google/osv.dev/blob/master/go/run_tests.sh
testutils dir https://github.com/google/osv.dev/tree/master/go/testutils
[0m
[0m$ [0mcurl -s "https://crt.sh/?q=%25.google.com" --max-time 60 | head -c 10000
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN">
<HTML>
<HEAD>
  <META http-equiv="Content-Type" content="text/html; charset=UTF-8">
  <TITLE>crt.sh | %.google.com</TITLE>
  <META name="description" content="Free CT Log Certificate Search Tool from Sectigo (formerly Comodo CA)">
  <META name="keywords" content="crt.sh, CT, Certificate Transparency, Certificate Search, SSL Certificate, Sectigo, Comodo CA">
  <LINK href="//fonts.googleapis.com/css2?family=DM+Sans:ital,wght@0,400;0,500;0,600;0,700;1,700&family=Urbanist:wght@700&family=Roboto+Mono&display=swap" rel="stylesheet">
  <STYLE type="text/css">
    a {
      color: #015258;
      text-decoration: underline;
      white-space: nowrap;
    }
    body {
      font: 12pt "DM Sans", sans-serif;
      padding-top: 10px;
      text-align: center
    }
    form {
      margin: 0px
    }
    span {
      border-radius: 10px
    }
    span.heading {
      color: #333132;
      font: 12pt "DM Sans", sans-serif
    }
    span.crtsh {
      background-color: #015258;
      color: #FFFFFF;
      font: bold 30px "Urbanist", sans-serif;
      padding: 2px 20px;
      border-radius: 6px
    }
    span.text {
      color: #888888;
      font: 10pt "DM Sans", sans-serif
    }
    span.title {
      color: #333132;
      font: bold 30px "Urbanist", sans-serif;
      padding: 10px 12px
    }
    .header {
      position: relative;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 32px;
      flex-wrap: wrap
    }
    .header-extras {
      position: absolute;
      left: calc(50% + 250px);
      top: 50%;
      transform: translateY(-50%);
      display: flex;
      align-items: center;
      gap: 24px
    }
    .header-icons {
      display: flex;
      align-items: center;
      gap: 16px
    }
    .header-icons A {
      display: inline-flex
    }
    .header-link {
      font: 8pt "DM Sans", sans-serif;
      color: #014149;
      text-decoration: underline
    }
    table {
      border-collapse: collapse;
      color: #222222;
      font: 10pt "DM Sans", sans-serif;
      margin-left: auto;
      margin-right: auto
    }
    table.options {
      border: none;
      margin-left: 10px
    }
    td, th {
      border: 1px solid #CCCCCC;
      padding: 0px 2px;
      text-align: left;
      vertical-align: top
    }
    td.outer, th.outer {
      border: 1px solid #CCCCCC;
      padding: 2px 20px;
      text-align: left
    }
    th.heading {
      color: #888888;
      font: bold italic 12pt "DM Sans", sans-serif;
      padding: 20px 0px 0px;
      text-align: center
    }
    th.options, td.options {
      border: none;
      vertical-align: middle
    }
    td.text {
      font: 10pt "Roboto Mono", monospace;
      padding: 2px 20px
    }
    table.identities td {
      font: 9pt "DM Mono", monospace;
      padding: 4px 8px
    }
    table.identities.compact td {
      font-size: 8pt;
      padding: 0 8px
    }
    table.identities.compact th {
      font-size: 9pt
    }
    td.heading {
      border: none;
      color: #888888;
      font: 12pt "DM Sans", sans-serif;
      padding-top: 20px;
      text-align: center
    }
    table.lint td, th {
      text-align: center
    }
    .button {
      background-color: #015258;
      border: none;
      border-radius: 1280px;
      color: #FFFFFF;
      cursor: pointer;
      font: 600 16px "DM Sans", sans-serif;
      padding: 4px 8px
    }
    .copyright {
      font: 8pt "DM Sans", sans-serif;
      color: #000000;
      text-align: center
    }
    .copyright a {
      color: #015258
    }
    .input {
      border: 1px solid #015258;
      border-radius: 6px;
      font: 500 14px "DM Sans", sans-serif;
      height: 44px;
      width: 528px;
      box-sizing: border-box;
      padding: 0 12px;
      text-align: center
    }
    .input:focus {
      border-color: #015258;
      outline: none
    }
    .small {
      font: 8pt "DM Sans", sans-serif;
      color: #888888
    }
    .error {
      background-color: #FFDFDF;
      color: #CC0000;
      font-weight: bold
    }
    .fatal {
      background-color: #0000AA;
      color: #FFFFFF;
      font-weight: bold
    }
    .notice {
      background-color: #FFFFDF;
      color: #606000
    }
    .warning {
      background-color: #FFEFDF;
      color: #DF6000
    }
    *:focus {
      outline: 0px transparent !important
    }
  </STYLE>
</HEAD>
<BODY>
  <DIV class="header">
    <A style="text-decoration:none" href="/"><SPAN class="crtsh">crt.sh</SPAN></A>  <SPAN class="title">Identity Search</SPAN>
  <DIV class="header-extras">
    <DIV class="header-icons">
      <A href="atom?q=google.com"><IMG src="/atom.svg" width="48" height="48"></A>
      <A href="csv?q=google.com"><IMG src="/csv.svg" width="48" height="48"></A>
      <A href="json?q=google.com"><IMG src="/json.svg" width="48" height="48"></A>
    </DIV>
    <A class="header-link" href="?q=google.com&dir=^&sort=1&group=icaid">Group by Issuer</A>
  </DIV>
  </DIV>
<BR>
<TABLE>
  <TR>
    <TH class="outer">Criteria</TH>
    <TD class="outer">Type: Identity&nbsp;&nbsp;&nbsp;&nbsp;Match: ILIKE&nbsp;&nbsp;&nbsp;&nbsp;Search: 'google.com'</TD>
  </TR>
</TABLE>
<BR>
<DIV style="color:#CC0000;padding-bottom:20px">
  <B>Sorry, your search results have been truncated.</B>
  <BR>It is not currently possible to sort and paginate large result sets efficiently, so only a random subset is shown below.  <BR>Please retry your search with <A href="?q=google.com&exclude=expired&group=none">expired certificates excluded</A>.
</DIV>
<TABLE>
  <TR>
    <TH class="outer">Certificates</TH>
    <TD class="outer">
<TABLE class="identities">
  <TR>
    <TH>
      <A href="?q=google.com&dir=v&sort=0&group=none">crt.sh ID</A>
    </TH>
    <TH style="white-space:nowrap">
      &nbsp;<A href="?q=google.com&dir=v&sort=1&group=none">Logged At</A>&nbsp;
 &#8679;    </TH>
    <TH style="white-space:nowrap"><A href="?q=google.com&dir=v&sort=2&group=none">Not Before</A>
    </TH>
    <TH style="white-space:nowrap"><A href="?q=google.com&dir=v&sort=4&group=none">Not After</A>
    </TH>
    <TH>Common Name</TH>
    <TH>Matching Identities</TH>
    <TH>
      <A href="?q=google.com&dir=v&sort=3&group=none">Issuer Name</A>
    </TH>
  </TR>
  <TR>
    <TD style="text-align:center"><A href="?id=3144337544">3144337544</A></TD>
    <TD style="text-align:center;white-space:nowrap">2020-07-26</TD>
    <TD style="text-align:center;white-space:nowrap">2011-07-10</TD>
    <TD style="text-align:center;white-space:nowrap">2013-07-09</TD>
    <TD style="white-space:nowrap">*.google.com</TD>
    <TD style="white-space:nowrap">admin@google.com<BR>*.google.com</TD>
    <TD><A style="white-space:normal" href="?caid=12254">C=NL, O=DigiNotar, CN=DigiNotar Public CA 2025, emailAddress=info@diginotar.nl</A></TD>
  </TR>

  <TR>
    <TD style="text-align:center"><A href="?id=2381394777">2381394777</A></TD>
    <TD style="text-align:center;white-space:nowrap">2020-01-27</TD>
    <TD style="text-align:center;white-space:nowrap">2011-07-13</TD>
    <TD style="text-align:center;white-space:nowrap">2012-07-13</TD>
    <TD style="white-space:nowrap">*.mail.google.com</TD>
    <TD style="white-space:nowrap">*.docs.google.com<BR>*.mail.google.com<BR>*.plus.google.com<BR>*.sites.google.com<BR>*.talkgadget.google.com</TD>
    <TD><A style="white-space:normal" href="?caid=4">C=US, O=Google Inc, CN=Google Internet Authority</A></TD>
  </TR>

  <TR>
    <TD style="text-align:center"><A href="?id=2380986199">2380986199</A></TD>
    <TD style="text-align:center;white-space:nowrap">2020-01-26</TD>
    <TD style="text-align:center;white-space:nowrap">2011-02-16</TD>
    <TD style="text-align:center;white-space:nowrap">2012-02-16</TD>
    <TD style="white-space:nowrap">*.mail.google.com</TD>
    <TD style="white-space:nowrap">*.docs.google.com<BR>*.mail.google.com<BR>*.sites.google.com<BR>*.talkgadget.google.com</TD>
    <TD><A style="white-space:normal" href="?caid=4">C=US, O=Google Inc, CN=Google Internet Authority</A></TD>
  </TR>

  <TR>
    <TD style="text-align:center"><A href="?id=2380850988">2380850988</A></TD>
    <TD style="text-align:center;white-space:nowrap">2020-01-26</TD>
    <TD style="text-align:center;white-space:nowrap">2012-02-29</TD>
    <TD style="text-align:center;white-space:nowrap">2013-02-28</TD>
    <TD style="white-space:nowrap">onex.wifi.google.com</TD>
    <TD style="white-space:nowrap">onex.wifi.google.com</TD>
    <TD><A style="white-space:normal" href="?caid=4">C=US, O=Google Inc, CN=Google Internet Authority</A></TD>
  </TR>

  <TR>
    <TD style="text-align:center"><A href="?id=2380841885">2380841885</A></TD>
    <TD style="text-align:center;white-space:nowrap">2020-01-26</TD>
    <TD style="text-align:center;white-space:nowrap">2011-07-13</TD>
    <TD style="text-align:center;white-space:nowrap">2012-07-13</TD>
    <TD style="white-space:nowrap">accounts.google.com</TD>
    <TD style="white-space:nowrap">accounts.google.com</TD>
    <TD><A style="white-space:normal" href="?caid=4">C=US, O=Google Inc, CN=Google Internet Authority</A></TD>
  </TR>

  <TR>
    <TD style="text-align:center"><A href="?id=2380681291">2380681291</A></TD>
    <TD style="text-align:center;white-space:nowrap">2020-01-26</TD>
    <TD style="text-align:center;white-space:nowrap">2013-11-22</TD>
    <TD style="text-align:center;white-space:nowrap">2013-11-24</TD>
    <TD style="white-space:nowrap">hosted-id.google.com</TD>
    <TD style="white-space:nowrap">hosted-id.google.com</TD>
    <TD><A style="white-space:normal" href="?caid=1348">C=US, O=Google Inc, CN=Google Internet Authority G2</A></TD>
  </TR>

  <TR>
    <TD style="text-align:center"><A href="?id=2380579544">2380579544</A></TD>
    <TD style="text-align:center;white-space:nowrap">2020-01-26</TD>
    <TD style="text-align:center;white-space:nowrap">2011-05-11</TD>
    <TD style="text-align:center;white-space:nowrap">2012-05-11</TD>
    <TD style="white-space:nowrap">acc
[0m
[0m$ [0mcurl -s "https://api.github.com/repos/google/osv-scanner/contents" | jq -r '.[] | "\(.name) \(.type) \(.html_url)"'
.dockerignore file https://github.com/google/osv-scanner/blob/main/.dockerignore
.editorconfig file https://github.com/google/osv-scanner/blob/main/.editorconfig
.gemini dir https://github.com/google/osv-scanner/tree/main/.gemini
.github dir https://github.com/google/osv-scanner/tree/main/.github
.gitignore file https://github.com/google/osv-scanner/blob/main/.gitignore
.golangci-lint-version file https://github.com/google/osv-scanner/blob/main/.golangci-lint-version
.golangci.yaml file https://github.com/google/osv-scanner/blob/main/.golangci.yaml
.goreleaser-nightly.yml file https://github.com/google/osv-scanner/blob/main/.goreleaser-nightly.yml
.goreleaser.yml file https://github.com/google/osv-scanner/blob/main/.goreleaser.yml
.pre-commit-hooks.yaml file https://github.com/google/osv-scanner/blob/main/.pre-commit-hooks.yaml
.prettierignore file https://github.com/google/osv-scanner/blob/main/.prettierignore
.prettierrc.json file https://github.com/google/osv-scanner/blob/main/.prettierrc.json
AGENTS.md file https://github.com/google/osv-scanner/blob/main/AGENTS.md
CHANGELOG.md file https://github.com/google/osv-scanner/blob/main/CHANGELOG.md
CONTRIBUTING.md file https://github.com/google/osv-scanner/blob/main/CONTRIBUTING.md
Dockerfile file https://github.com/google/osv-scanner/blob/main/Dockerfile
LICENSE file https://github.com/google/osv-scanner/blob/main/LICENSE
Makefile file https://github.com/google/osv-scanner/blob/main/Makefile
README.md file https://github.com/google/osv-scanner/blob/main/README.md
action.dockerfile file https://github.com/google/osv-scanner/blob/main/action.dockerfile
actions dir https://github.com/google/osv-scanner/tree/main/actions
cmd dir https://github.com/google/osv-scanner/tree/main/cmd
docs dir https://github.com/google/osv-scanner/tree/main/docs
exit_code_redirect.sh file https://github.com/google/osv-scanner/blob/main/exit_code_redirect.sh
go.mod file https://github.com/google/osv-scanner/blob/main/go.mod
go.sum file https://github.com/google/osv-scanner/blob/main/go.sum
goreleaser-action.dockerfile file https://github.com/google/osv-scanner/blob/main/goreleaser-action.dockerfile
goreleaser.dockerfile file https://github.com/google/osv-scanner/blob/main/goreleaser.dockerfile
internal dir https://github.com/google/osv-scanner/tree/main/internal
llms.txt file https://github.com/google/osv-scanner/blob/main/llms.txt
osv-scanner.toml file https://github.com/google/osv-scanner/blob/main/osv-scanner.toml
pkg dir https://github.com/google/osv-scanner/tree/main/pkg
renovate.json file https://github.com/google/osv-scanner/blob/main/renovate.json
scripts dir https://github.com/google/osv-scanner/tree/main/scripts
[0m
[0m$ [0mcurl -s "https://api.github.com/repos/google/atheris/contents" | jq -r '.[] | "\(.name) \(.type) \(.html_url)"'
.github dir https://github.com/google/atheris/tree/master/.github
.gitignore file https://github.com/google/atheris/blob/master/.gitignore
.kokoro dir https://github.com/google/atheris/tree/master/.kokoro
CONTRIBUTING.md file https://github.com/google/atheris/blob/master/CONTRIBUTING.md
LICENSE file https://github.com/google/atheris/blob/master/LICENSE
MANIFEST.in file https://github.com/google/atheris/blob/master/MANIFEST.in
README.md file https://github.com/google/atheris/blob/master/README.md
atheris_no_libfuzzer.py file https://github.com/google/atheris/blob/master/atheris_no_libfuzzer.py
contrib dir https://github.com/google/atheris/tree/master/contrib
deployment dir https://github.com/google/atheris/tree/master/deployment
example_fuzzers dir https://github.com/google/atheris/tree/master/example_fuzzers
hall_of_fame.md file https://github.com/google/atheris/blob/master/hall_of_fame.md
hooking.md file https://github.com/google/atheris/blob/master/hooking.md
libfuzzer_mod dir https://github.com/google/atheris/tree/master/libfuzzer_mod
native_extension_fuzzing.md file https://github.com/google/atheris/blob/master/native_extension_fuzzing.md
pyproject.toml file https://github.com/google/atheris/blob/master/pyproject.toml
run_tests.sh file https://github.com/google/atheris/blob/master/run_tests.sh
setup.cfg file https://github.com/google/atheris/blob/master/setup.cfg
setup.py file https://github.com/google/atheris/blob/master/setup.py
setup_utils dir https://github.com/google/atheris/tree/master/setup_utils
src dir https://github.com/google/atheris/tree/master/src
[0m
[0m$ [0mcurl -s "https://crt.sh/?q=google.com&output=csv&exclude=expired" --max-time 60 | head -200
(no output)
[0m
[0m$ [0mcurl -s "https://bughunters.google.com/about/rules/5572729039880192" -L | head -c 10000
<!doctype html>
<html lang="en-US">
  <head>
    <meta charset="utf-8" />
    <title>Google Bug Hunters</title>
    <meta property="twitter:card" content="summary" />
    <meta property="twitter:site" content="GoogleVRP" />
    <meta property="og:title" content="Google Bug Hunters" />
	<meta name="description" content="Welcome to Google&#39;s Bug Hunting community, learn more about hunting &amp; reporting bugs you’ve found in Google products." />
	<meta property="og:description" content="Welcome to Google&#39;s Bug Hunting community, learn more about hunting &amp; reporting bugs you’ve found in Google products." />
	<meta property="og:image" content="https://storage.googleapis.com/bughunters-social/og_social_image_bughunters.png" />
	<meta property="og:url" content="https://bughunters.google.com/about/rules/5572729039880192" />
	<meta property="twitter:title" content="Google Bug Hunters" />
	<meta property="twitter:description" content="Welcome to Google&#39;s Bug Hunting community, learn more about hunting &amp; reporting bugs you’ve found in Google products." />
	<meta property="twitter:image" content="https://storage.googleapis.com/bughunters-social/og_social_image_bughunters.png" />
    <base href="/" />

    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <link
      rel="icon"
      type="image/png"
      href="https://bughunters.google.com/static/icon-192.png"
      sizes="192x192"
    />
    <link
      rel="apple-touch-icon"
      type="image/png"
      href="https://bughunters.google.com/static/touch-icon-180.png"
      sizes="180x180"
    />
    <link rel="alternate" type="application/rss+xml" title="Security Engineering Blog" href="/feed/en" />

    
    <link
      href="https://fonts.googleapis.com/css2?family=Google+Sans:wght@400;500&family=Roboto+Mono:wght@400&family=Material+Icons&family=Material+Icons+Outlined&display=swap"
      rel="stylesheet"
    />

    
    <link
      href="https://www.gstatic.com/glue/cookienotificationbar/cookienotificationbar.min.css"
      rel="stylesheet"
    />
    <link rel="stylesheet" href="https://www.gstatic.com/bughunters/960149923/static/css/third_party_css-bundle.css" />

    
    <link rel="stylesheet" href="https://www.gstatic.com/bughunters/960149923/static/css/styles.css" />

    
    <script id="gtm-init" nonce="j0yELVxnoD2AefM7PM0Osk8Zylg=">
      window.dataLayer = window.dataLayer || [];
      function gtag() {
        dataLayer.push(arguments);
      }

      // The cookie consent banner library invokes this callback after it has fully loaded and set the
      // default consent state in dataLayer of `denied` and updated based on local storage settings . This timing prevents GTM from firing tags before consent `defaults`
      // of denied are in place.
      function glueCookieNotificationBarLoaded() {
        var gtmNonce = document.getElementById("gtm-init").nonce;
        var script = document.createElement("script");
        script.async = true;
        script.src = "https://www.googletagmanager.com/gtag/js";
        script.nonce = gtmNonce;
        document.head.appendChild(script);
      }
    </script>
  </head>
  <body class="mat-app-background">
    <app-root></app-root>
    <script
      nonce="j0yELVxnoD2AefM7PM0Osk8Zylg="
      defer
      src="https://www.gstatic.com/bughunters/960149923/static/js/third_party_js-bundle.js"
    ></script>
    <script nonce="j0yELVxnoD2AefM7PM0Osk8Zylg=" defer src="https://www.gstatic.com/bughunters/960149923/app_bundle_prod.js"></script>
  </body>
</html>
[0m
[0m$ [0mcurl -s "https://api.github.com/repos/google/guava/contents" | jq -r '.[] | "\(.name) \(.type) \(.html_url)"' | head -30
.gitattributes file https://github.com/google/guava/blob/master/.gitattributes
.github dir https://github.com/google/guava/tree/master/.github
.gitignore file https://github.com/google/guava/blob/master/.gitignore
.mvn dir https://github.com/google/guava/tree/master/.mvn
CONTRIBUTING.md file https://github.com/google/guava/blob/master/CONTRIBUTING.md
CONTRIBUTORS file https://github.com/google/guava/blob/master/CONTRIBUTORS
LICENSE file https://github.com/google/guava/blob/master/LICENSE
README.md file https://github.com/google/guava/blob/master/README.md
android dir https://github.com/google/guava/tree/master/android
cycle_suppress_list.txt file https://github.com/google/guava/blob/master/cycle_suppress_list.txt
futures dir https://github.com/google/guava/tree/master/futures
guava-bom dir https://github.com/google/guava/tree/master/guava-bom
guava-gwt dir https://github.com/google/guava/tree/master/guava-gwt
guava-testlib dir https://github.com/google/guava/tree/master/guava-testlib
guava-tests dir https://github.com/google/guava/tree/master/guava-tests
guava dir https://github.com/google/guava/tree/master/guava
integration-tests dir https://github.com/google/guava/tree/master/integration-tests
mvnw file https://github.com/google/guava/blob/master/mvnw
mvnw.cmd file https://github.com/google/guava/blob/master/mvnw.cmd
overview.html file https://github.com/google/guava/blob/master/overview.html
pom.xml file https://github.com/google/guava/blob/master/pom.xml
proguard dir https://github.com/google/guava/tree/master/proguard
util dir https://github.com/google/guava/tree/master/util
[0m
[0m$ [0mcurl -s "https://crt.sh/?q=google.com&output=json&exclude=expired" --max-time 60 | python3 -c "
import sys, json
try:
    data = json.load(sys.stdin)
    for item in data:
        name = item.get('name_value', '')
        if name:
            for n in name.split('\n'):
                n = n.strip()
                if n and 'google.com' in n:
                    print(n)
except:
    pass
" | sort -u | head -200
(no output)
[0m
[0m$ [0mcurl -s "https://r.jina.ai/http://bughunters.google.com/about/rules" | head -c 15000
Title: Rules | Google Bug Hunters

URL Source: http://bughunters.google.com/about/rules

Markdown Content:
1 showValues
*   [About This Section](https://bughunters.google.com/about/rules/about-this-section)
*   [Android & Friends](https://bughunters.google.com/about/rules/about-this-section#android-friends)
*   [Chrome & Friends](https://bughunters.google.com/about/rules/about-this-section#chrome-friends)
*   [Google & Friends](https://bughunters.google.com/about/rules/about-this-section#google-friends)
*   [Open Source](https://bughunters.google.com/about/rules/about-this-section#open-source)
*   [Other](https://bughunters.google.com/about/rules/about-this-section#other)

## About This Section

Welcome to the VRP rules section! Here, you’ll find an overview of the different vulnerability reward programs Google maintains, as well as guidance on related programs. See the below sections for an overview of the available programs, their scope, and where to report issues or submit contributions. Happy bug hunting!

If you have questions related to our handling of submitted security reports or the general functionality of the [bughunters.google.com](http://bughunters.google.com/) site, see our [FAQ page](https://bughunters.google.com/about/4925519884451840).

Tip: Not sure which program to report the issue you've discovered to? When in doubt, report to the [Google and Alphabet Vulnerability Reward Program (VRP)](https://bughunters.google.com/about/rules/6744710187712512/about-this-section#google-amp-friends).

## Android & Friends [](https://bughunters.google.com/about/rules/about-this-section#android-friends)

| Program name | Scope | Where to report |
| --- | --- | --- |
| Android and Google Devices Security Reward Program [(rules)](https://bughunters.google.com/about/rules/6171833274204160/android-and-google-devices-security-reward-program-rules) | Security issues affecting Pixel, Smart Home, Google Nest, Home APIs, Pixel Watch, and Fitbit devices and their latest operating systems | Use the [standard form](https://bughunters.google.com/report/vrp) (report to _Android & Devices VRP_) |
| Google Mobile Vulnerability Reward Program [(rules)](https://bughunters.google.com/about/rules/6618732618186752/google-mobile-vulnerability-reward-program-rules) | Security issues affecting first-party Android applications | Use the [standard form](https://bughunters.google.com/report/vrp) (report to _Mobile VRP_) |

## Chrome & Friends [](https://bughunters.google.com/about/rules/about-this-section#chrome-friends)

| Program name | Scope | Where to report |
| --- | --- | --- |
| Chrome Extensions Vulnerability Reward Program [(rules)](https://bughunters.google.com/about/rules/5668215344988160/chrome-extensions-vulnerability-reward-program-rules) | Security issues in first-party Chrome extensions | Use the [standard form](https://bughunters.google.com/report/vrp) (report to _Chrome Extensions VRP_) |
| Chrome Vulnerability Reward Program [(rules)](https://bughunters.google.com/about/rules/5745167867576320/chrome-vulnerability-reward-program-rules) | Security issues affecting the Chrome Browser | Use the Chromium issue tracker [submission form](https://issues.chromium.org/issues/new?noWizard=true&component=1363614&template=1922342) Alternatively, you can report issues via the [standard form](https://bughunters.google.com/report/vrp) (report to _Chrome VRP_) |
| ChromeOS Vulnerability Reward Program [(rules)](https://bughunters.google.com/about/rules/4919474699501568/chromeos-vulnerability-reward-program-rules) | Security issues affecting Chromebooks and the ChromeOS ecosystem | Use the [standard form](https://bughunters.google.com/report/vrp) (report to _ChromeOS VRP_) |

## Google & Friends [](https://bughunters.google.com/about/rules/about-this-section#google-friends)

| Program name | Scope | Where to report |
| --- | --- | --- |
| Abuse Vulnerability Reward Program [(rules)](https://bughunters.google.com/about/rules/5238081279623168/abuse-vulnerability-reward-program-rules) | Issues enabling abuse in a Google-owned or Alphabet (Bet) subsidiary web service that handles reasonably sensitive user data | Use the [standard form](https://bughunters.google.com/report/vrp) (report to _Abuse VRP_) |
| AI Vulnerability Reward Program [(rules)](https://bughunters.google.com/about/rules/5222232590712832/ai-vulnerability-reward-program-rules) | Security and abuse issues in a Google-owned or Alphabet (Bet) subsidiary AI-based product or service that handles reasonably sensitive user data | Use the [standard form](https://bughunters.google.com/report/vrp) (report to _AI VRP_) |
| Cloud Vulnerability Reward Program [(rules)](https://bughunters.google.com/about/rules/4849867320328192/cloud-vulnerability-reward-program-rules) | Security issues affecting any Google Cloud product or web service that handles reasonably sensitive user data | Use the [standard form](https://bughunters.google.com/report/vrp) (report to _Cloud VRP_) |
| Google and Alphabet Vulnerability Reward Program (VRP) [(rules)](https://bughunters.google.com/about/rules/6625378258649088/google-and-alphabet-vulnerability-reward-program-vrp-rules) | Security issues affecting any Google-owned or Alphabet (Bet) subsidiary web service that handles reasonably sensitive user data | Use the [standard form](https://bughunters.google.com/report/vrp) (report to _Google VRP_) |

## Open Source [](https://bughunters.google.com/about/rules/about-this-section#open-source)

| Program name | Scope | Where to report |
| --- | --- | --- |
| Google Open Source Software Vulnerability Reward Program [(rules)](https://bughunters.google.com/about/rules/6521337925468160/google-open-source-software-vulnerability-reward-program-rules) | Security issues affecting open source software stored in the public repositories of Google-owned GitHub organizations and selected repositories hosted on other platforms | Use the [standard form](https://bughunters.google.com/report/vrp) (report to _OSS VRP_) |
| Patch Rewards Program [(rules)](https://bughunters.google.com/about/rules/4928084514701312/patch-rewards-program-rules) | Rewards proactive improvements you've made to security in open source projects | Submit patches using the [dedicated Patch Rewards form](https://bughunters.google.com/report/patch_rewards) |
| Tsunami Patch Rewards Program [(rules)](https://bughunters.google.com/about/rules/open-source/5067456626688000/tsunami-patch-rewards-program-rules) | As of April 9, 2026, this program is in on hold for the foreseeable future and currently not accepting new requests ([details](https://github.com/google/tsunami-security-scanner-plugins/issues/819)). Rewards contributions to Tsunami security scanners which enhance vulnerability detection and web application fingerprinting capabilities | Submit a request using the [dedicated Tsunami form](https://bughunters.google.com/report/tsunami); note that prior approval by the Tsunami scanner team is required ([full application process](https://bughunters.google.com/about/rules/open-source/5067456626688000/tsunami-patch-rewards-program-rules#application-process)) |
| InternetCTF Tsunami Patch Rewards Program [(rules)](https://bughunters.google.com/about/rules/open-source/5067456626688000/tsunami-patch-rewards-program-rules#internetctf-patch-reward-process) | As of April 9, 2026, this program is in on hold for the foreseeable future and currently not accepting new requests ([details](https://github.com/google/tsunami-security-scanner-plugins/issues/819)). Rewards contributions to discovering & reporting 0-day vulnerabilities in open source software and implementing Tsunami plugins to detect them | Exfiltrate flags at [InternetCTF](https://capturetheflag.withgoogle.com/internet) and fill in the [survey](https://docs.google.com/forms/d/e/1FAIpQLSesBR3zNpaXaZ3vgN2lc1ulclaBjH-ZBSuDS9Hsyf4T8XHT9g/viewform?usp=sf_link) for vulnerability report and Tsunami plugin implementation ([full application process](https://bughunters.google.com/about/rules/open-source/5067456626688000/tsunami-patch-rewards-program-rules#internetctf-patch-reward-process)) |
| OSV-SCALIBR Patch Rewards Program Rules [(rules)](https://bughunters.google.com/about/rules/open-source/6436351477940224/osv-scalibr-patch-rewards-program-rules) | As of July 6, 2026, this program is in on hold for the foreseeable future and currently not accepting new requests ([details](https://github.com/google/osv-scalibr/issues/1949)). Rewards contributions to OSV-SCALIBR, Google's filesystem scanner for vulnerability detection and identifying software inventory extraction capabilities | Submit a request using the [dedicated OSV-SCALIBR form](https://bughunters.google.com/report/tsunami); note that prior approval by the OSV-SCALIBR team is required ([full application process](https://bughunters.google.com/about/rules/open-source/6436351477940224/osv-scalibr-patch-rewards-program-rules#application-process)) |

## Other [](https://bughunters.google.com/about/rules/about-this-section#other)

| Program/topic name | Scope | Where to report |
| --- | --- | --- |
| Bonus Awards [(rules)](https://bughunters.google.com/about/rules/5429687846305792/bonus-awards-rules) | Time-limited (extra) bonuses for reports to specific VRP targets | Depends on the target the reward is available for |
| Our Rewards Philosophy [(rules)](https://bughunters.google.com/about/rules/4737983938560000/our-rewards-philosophy) | Provides background on how we evaluate submitted reports and determine rewards | n/a |
| Vulnerability Research Grant [(rules)](https://bughunters.google.com/about/rules/5479188746993664/vulnerability-research-grant-rules) | Provides upfront grants to encourage security research in a pre-defined area | n/a |
| Code of Conduct [(rules)](https://bughunters.google.com/about/rules/other/6009584292331520/code-of-conduct-for-our-vulnerability-reward-programs) | Outlines the standards of behavior expected from the community of researchers participating in our VRPs and the Google teams interacting with them | n/a |
[0m
[0m$ [0mcurl -s "https://api.github.com/repos/googleprojectzero" 2>/dev/null | head -c 1000
{
  "message": "Not Found",
  "documentation_url": "https://docs.github.com/rest",
  "status": "404"
}
[0m
[0m$ [0mcurl -s "https://crt.sh/?q=google.com&output=json" --max-time 60 | python3 -c "
import sys, json
try:
    data = json.load(sys.stdin)
    print(f'Total: {len(data)}')
    for item in data[:20]:
        print(item.get('name_value', 'N/A')[:200])
except Exception as e:
    print(f'Error: {e}')
    # Print raw response for debugging
    sys.stdin.seek(0)
    raw = sys.stdin.read(500)
    print(f'Raw: {raw}')
"
Total: 1152
admin@google.com
*.google.com
*.docs.google.com
*.mail.google.com
*.plus.google.com
*.sites.google.com
*.talkgadget.google.com
*.docs.google.com
*.mail.google.com
*.sites.google.com
*.talkgadget.google.com
onex.wifi.google.com
accounts.google.com
hosted-id.google.com
accounts.google.com
adwords.google.com
adwords.google.com.ar
adwords.google.com.au
adwords.google.com.br
adwords.google.com.cn
adwords.google.com.gr
adwords.google.com.hk
adwords.google.com.ly
adwords.google.com.mx
adwor
adwords.google.com
adwords.google.com.ar
adwords.google.com.au
adwords.google.com.br
adwords.google.com.cn
adwords.google.com.gr
adwords.google.com.hk
adwords.google.com.ly
adwords.google.com.mx
adwor
adwords.google.com
adwords.google.com.ar
adwords.google.com.au
adwords.google.com.br
adwords.google.com.cn
adwords.google.com.gr
adwords.google.com.hk
adwords.google.com.ly
adwords.google.com.mx
adwor
*.vp.video.l.google.com
adwords.google.com
adwords.google.com.ar
adwords.google.com.au
adwords.google.com.br
adwords.google.com.cn
adwords.google.com.gr
adwords.google.com.hk
adwords.google.com.ly
adwords.google.com.mx
adwor
adwords.google.com
adwords.google.com.ar
adwords.google.com.au
adwords.google.com.br
adwords.google.com.cn
adwords.google.com.gr
adwords.google.com.hk
adwords.google.com.ly
adwords.google.com.mx
adwor
adwords.google.com
adwords.google.com.ar
adwords.google.com.au
adwords.google.com.br
adwords.google.com.cn
adwords.google.com.gr
adwords.google.com.hk
adwords.google.com.ly
adwords.google.com.mx
adwor
accounts.google.com
wifi.google.com
jmt0.google.com
wifi.google.com
jmt0.google.com
*.docs.google.com
upload.video.google.com
[0m
[0m$ [0mcurl -s "https://crt.sh/?q=google.com&output=json" --max-time 60 | python3 -c "
import sys, json
try:
    data = json.load(sys.stdin)
    subdomains = set()
    for item in data:
        name = item.get('name_value', '')
        if name:
            for n in name.split('\n'):
                n = n.strip().lower()
                if n and 'google.com' in n and not n.startswith('*.'):
                    subdomains.add(n)
    for s in sorted(subdomains):
        print(s)
except Exception as e:
    print(f'Error: {e}')
" | head -300
Error: Expecting value: line 1 column 1 (char 0)
[0m
[0m$ [0mcurl -s "https://api.github.com/repos/google/tsunami-security-scanner/contents" | jq -r '.[] | "\(.name) \(.type) \(.html_url)"'
.dockerignore file https://github.com/google/tsunami-security-scanner/blob/master/.dockerignore
.gitattributes file https://github.com/google/tsunami-security-scanner/blob/master/.gitattributes
.github dir https://github.com/google/tsunami-security-scanner/tree/master/.github
.gitignore file https://github.com/google/tsunami-security-scanner/blob/master/.gitignore
LICENSE file https://github.com/google/tsunami-security-scanner/blob/master/LICENSE
README.md file https://github.com/google/tsunami-security-scanner/blob/master/README.md
build.gradle file https://github.com/google/tsunami-security-scanner/blob/master/build.gradle
common dir https://github.com/google/tsunami-security-scanner/tree/master/common
core.Dockerfile file https://github.com/google/tsunami-security-scanner/blob/master/core.Dockerfile
devel.Dockerfile file https://github.com/google/tsunami-security-scanner/blob/master/devel.Dockerfile
docs dir https://github.com/google/tsunami-security-scanner/tree/master/docs
full.Dockerfile file https://github.com/google/tsunami-security-scanner/blob/master/full.Dockerfile
go.mod file https://github.com/google/tsunami-security-scanner/blob/master/go.mod
main dir https://github.com/google/tsunami-security-scanner/tree/master/main
plugin dir https://github.com/google/tsunami-security-scanner/tree/master/plugin
plugin_server dir https://github.com/google/tsunami-security-scanner/tree/master/plugin_server
proto dir https://github.com/google/tsunami-security-scanner/tree/master/proto
settings.gradle file https://github.com/google/tsunami-security-scanner/blob/master/settings.gradle
tsunami.yaml file https://github.com/google/tsunami-security-scanner/blob/master/tsunami.yaml
tsunami_tcs.yaml file https://github.com/google/tsunami-security-scanner/blob/master/tsunami_tcs.yaml
workflow dir https://github.com/google/tsunami-security-scanner/tree/master/workflow
[0m
[0m$ [0mcurl -s "https://r.jina.ai/http://bughunters.google.com/about/rules/6625378258649088/google-and-alphabet-vulnerability-reward-program-vrp-rules" | head -c 15000
Title: Google and Alphabet Vulnerability Reward Program (VRP) Rules | Google Bug Hunters

URL Source: http://bughunters.google.com/about/rules/6625378258649088/google-and-alphabet-vulnerability-reward-program-vrp-rules

Markdown Content:
We have long enjoyed a close relationship with the security research community. To honor all the cutting-edge external contributions that help us keep our users safe, we maintain a Vulnerability Reward Program for Google-owned and Alphabet (Bet) subsidiary web properties, running continuously since November 2010.

## Services in scope [](https://bughunters.google.com/about/rules/google-friends/google-and-alphabet-vulnerability-reward-program-vrp-rules#services-in-scope)

In principle, any Google-owned or Alphabet (Bet) subsidiary web service that handles reasonably sensitive user data is intended to be in scope. This includes virtually all the content in the following domains:

*   *.google.com
*   *.youtube.com
*   *.blogger.com
*   *.deepmind.com
*   *.waymo.com
*   *.wing.com

Bugs in Google- and Waymo-developed apps published in the [Apple App Store](https://apps.apple.com/developer/google-inc/id281956209) will also qualify. For information on further services and devices that are in scope of different reward programs, see the rules for the following programs:

*   [Abuse Vulnerability Reward Program Rules](https://bughunters.google.com/about/rules/5238081279623168/abuse-vulnerability-reward-program-rules)
*   [AI Vulnerability Reward Program Rules](https://bughunters.google.com/about/rules/5222232590712832/ai-vulnerability-reward-program-rules)
*   [Android and Google Devices Security Reward Program Rules](https://bughunters.google.com/about/rules/6171833274204160/android-and-google-devices-security-reward-program-rules)
*   [Chrome Vulnerability Reward Program Rules](https://bughunters.google.com/about/rules/5745167867576320/chrome-vulnerability-reward-program-rules)
*   [ChromeOS Vulnerability Reward Program Rules](https://bughunters.google.com/about/rules/4919474699501568/chromeos-vulnerability-reward-program-rules)
*   [Cloud Vulnerability Reward Program Rules](https://bughunters.google.com/about/rules/4849867320328192/cloud-vulnerability-reward-program-rules)
*   [Google Mobile Vulnerability Reward Program Rules](https://bughunters.google.com/about/rules/6618732618186752/google-mobile-vulnerability-reward-program-rules)
*   [Google Open Source Software Vulnerability Reward Program Rules](https://bughunters.google.com/about/rules/6521337925468160/google-open-source-software-vulnerability-reward-program-rules)
*   [Chrome Extensions Vulnerability Reward Program Rules](https://bughunters.google.com/about/rules/5668215344988160/chrome-extensions-vulnerability-reward-program-rules)
*   [Verily Bug Bounty Program Rules on HackerOne](https://hackerone.com/verily_life_sciences)

On the flip side, the program has two important exclusions to keep in mind:

*   **Third-party websites** – Some Google-branded services hosted in less common domains may be operated by our vendors or partners. We can't authorize you to test these systems on behalf of their owners and will not reward such reports. Please read the fine print on the page and examine domain and IP WHOIS records to confirm. If in doubt, talk to us first!
*   **Recent acquisitions** – To allow time for internal review and remediation, newly acquired companies are subject to a six-month blackout period. Bugs reported sooner than that will typically not qualify for a reward.

## Qualifying vulnerabilities [](https://bughunters.google.com/about/rules/google-friends/google-and-alphabet-vulnerability-reward-program-vrp-rules#qualifying-vulnerabilities)

Any design or implementation issue that substantially affects the confidentiality or integrity of user data is likely to be in scope for the program. Common examples include:

*   Cross-site scripting,
*   Cross-site request forgery,
*   Mixed-content scripts,
*   Authentication or authorization flaws,
*   Server-side code execution bugs,
*   [XSLeak bugs](https://bughunters.google.com/learn/invalid-reports/web-platform/xsleaks/5022006283862016/xsleaks-and-xs-search).

Note that the scope of the program is limited to technical vulnerabilities in Google-owned browser extensions, mobile, and web applications; please do not try to sneak into Google offices, attempt phishing attacks against our employees, and so on.

Out of concern for the availability of our services to all users, please do not attempt to carry out DoS attacks, leverage black hat SEO techniques, spam people, or do other similarly questionable things. We also discourage the use of any vulnerability testing tools that automatically generate very significant volumes of traffic.

## Non-qualifying vulnerabilities [](https://bughunters.google.com/about/rules/google-friends/google-and-alphabet-vulnerability-reward-program-vrp-rules#non-qualifying-vulnerabilities)

**Note:** Visit our [Bug Hunter University](https://bughunters.google.com/learn) page dedicated to common non-qualifying findings and vulnerabilities.

Depending on their impact, some of the reported issues may not qualify. Although we review them on a case-by-case basis, here are some of the common low-risk issues that typically do not earn a monetary reward:

*   **Vulnerabilities in *.bc.googleusercontent.com or *.appspot.com.** These domains are used to host applications that belong to Google Cloud customers. The Vulnerability Reward Program does not authorize the testing of Google Cloud customer applications. Google Cloud customers can authorize the penetration testing of their own applications ([_read more_](https://cloud.google.com/security/)), but testing of these domains is not within the scope of or authorized by the Vulnerability Reward Program.
*   **Cross-site scripting vulnerabilities in “sandbox” domains** ([_read more_](https://bughunters.google.com/learn/invalid-reports/web-platform/xss/6619189462433792).) We maintain a number of domains that leverage the same-origin policy to safely isolate certain types of untrusted content; the most prominent example of this is *.googleusercontent.com. Unless an impact on sensitive user data can be demonstrated, we do not consider the ability to execute JavaScript in that domain to be a bug.
*   **Execution of owner-supplied JavaScript in Blogger.** Blogs hosted in *.blogspot.com are no different from any third-party website on the Internet. For your safety, we employ spam and malware detection tools, but we do not consider the ability to embed JavaScript within your own blog to be a security bug.
*   **URL redirection** ([_read more_](https://bughunters.google.com/learn/invalid-reports/navigation/6680364896223232).) We recognize that the address bar is the only reliable security indicator in modern browsers; consequently, we hold that the usability and security benefits of a small number of well-designed and closely monitored redirectors outweigh their true risks.
*   **Legitimate content proxying and framing.** We expect our services to unambiguously label third-party content and to perform a number of abuse-detection checks, but as with redirectors, we think that the value of products such as Google Translate outweighs the risk.
*   **Bugs requiring exceedingly unlikely user interaction.** For example, a cross-site scripting flaw that requires the victim to manually type in an XSS payload into Google Maps and then double-click an error message may realistically not meet the bar.
*   **Logout cross-site request forgery** ([_read more_](https://bughunters.google.com/learn/invalid-reports/web-platform/csrf-clickjacking/5072689380982784).) For better or worse, the design of HTTP cookies means that no single website can prevent its users from being logged out; consequently, application-specific ways of achieving this goal will likely not qualify. You may be interested in personal blog posts from [Chris Evans](http://scarybeastsecurity.blogspot.com/2010/01/logout-xsrf-significant-web-app-bug.html) and [Michal Zalewski](http://lcamtuf.blogspot.com/2010/10/http-cookies-or-how-not-to-design.html) for more background.
*   **Flaws affecting the users of out-of-date browsers and plugins.** The security model of the web is constantly being fine-tuned. The panel typically does not reward reports that describe issues that affect only the users of outdated or unpatched browsers.
*   **Presence of banner or version information.** Version information does not, by itself, expose the service to attacks - so we do not consider this to be a bug. That said, if you find outdated software and have good reasons to suspect that it poses a well-defined security risk, please let us know.
*   **Email spoofing on Gmail and Google Groups.** We are aware of the risk presented by spoofed messages and are taking steps to ensure that the Gmail filter can effectively deal with such attacks.
*   **User enumeration.** Reports outlining user enumeration are not within scope unless you can demonstrate that we don't have any rate limits in place to protect our users.
*   **Bypassing the limit of accounts that can be verified with a given SMS number.** We often receive reports about users being able to bypass our SMS limit for verifying accounts. There are actually two different quotas per number for account verification, one via 'SMS' and a different one via 'Call Me'.

Monetary rewards aside, vulnerability reporters who work with us to resolve security bugs in our products will be credited on the [Leaderboard](https://bughunters.google.com/leaderboard). If we file an internal security bug, we will acknowledge your contribution on that page.

## Reward amounts for security vulnerabilities [](https://bughunters.google.com/about/rules/google-friends/google-and-alphabet-vulnerability-reward-program-vrp-rules#reward-amounts-for-security-vulnerabilities)

When determining the reward amount for a reported vulnerability, two central factors are taken into account:

*   The [Domain Tier](https://github.com/google/bughunters/blob/main/domain-tiers/external_domains_google.asciipb) of the application in which the vulnerability was identified
*   The specific category and impact of the vulnerability, which in many cases is linked to the Information Tier / Action Criticality

### Information tiers (IT) [](https://bughunters.google.com/about/rules/google-friends/google-and-alphabet-vulnerability-reward-program-vrp-rules#information-tiers-it-)

Each information tier is aimed to reflect the sensitivity of the user's information and the potential consequences to the user in case of that information being leaked – with Tier 0 being the most sensitive and Tier 2 the least sensitive.

**Tier 0 - Credentials and sensitive data from internal systems** (expand for details)
**Tier 0** information is a type of information that could be used to compromise a user's Google Account through state-changing actions (for example an XSRF token for changing a user's password), to expose credentials (such as security tokens, passwords), or to compromise our infrastructure or users’ data (e.g. internal API keys, vulnerabilities in our issue tracker) in a significant way across multiple Google systems and applications.

Examples:

*   **Contents of a user’s emails:** Emails could contain recovery tokens that can be used to compromise a user’s account
*   **Credentials:** For example, Oauth tokens to Google applications, or Google Account passwords
*   **Information from internal systems that could leak information that allows account takeover** (e.g. vulnerabilities in our issue tracker, in-progress incidents, etc.)
*   **Internal API keys:** Some internal API keys could be used to access information from any Google Account, for example hardcoded API keys embedded into AI agents. Note that reports concerning API keys are frequently false positives – always verify your findings before reporting them to us (see [Understanding API key leaks](https://bughunters.google.com/learn/invalid-reports/google-products/understanding-api-key-leaks) for more information)
*   **Information leak leading to business account takeover**, e.g. account takeover of a Google Workspace Admin

**Tier 1 - High security impact user data and internal system data** (expand for details)
**Tier 1** information includes user data (e.g. Drive documents, photos), sensitive personal information (e.g. a user’s health records), or a collection of multiple pieces of Tier 2 information.

Tier 1 also includes sensitive internal system data, but which cannot be used to compromise our infrastructure or impact user data (in which case that data would likely be Information Tier 0).

Examples:

*   **Drive document contents, Keep notes**
*   **Contents of messages, address book, calendar events (if private)**
*   **Photos, saved places in Maps**
*   **Other data** which typically is meant to be private and which is stored by the user in a Google application fit for storing such data
*   **Account credentials for 3rd-party applications**, e.g. GitHub API keys or non-core Google applications hosted on Domain Tier 2 and lower domains such as GFiber. Note that reports concerning API keys are frequently false positives – always verify your findings before reporting them to us (see [Understanding API key leaks](https://bughunters.google.com/learn/invalid-reports/google-products/understanding-api-key-leaks) for more information)
*   **Business-critical data about Google customers or external applications**, e.g. substantial information about unreleased products
*   **Sensitive user data** such as payments information, fine-grained user location data, Government Identification Numbers etc.
*   Large and non-trivial portion of the **server-side codebase of sensitive internal applications or systems**

**Tier 2 - Metadata & other lower security impact user data and internal product data** (expand for details)
**Tier 2** information is metadata or user data we consider to have less security impact or less sensitivity than IT1 data.

Tier 2 also includes less sensitive data from internal systems that nevertheless has a security impact.

Tier 2 does **not** include data _without_ a security impact such as leaking internal domain names, error stacktraces, internal error messages, or version banners.

Examples:

*   **Titles of only a few private documents**
*   **Email address / Gaia ID / Name** (unless meant to be [public](https://support.google.com/accounts/answer/6304920?hl=en&co=GENIE.Platform%253DAndroid%2523zippy=%252Cwhat-info-can-be-shown&sa=D&source=docs&ust=1770732659114970&usg=AOvVaw1OJmYVE0q7d4ZXXru7evrL#zippy=%2Cwhat-info-can-be-shown:~:text=Here%E2%80%99s%20some%20info%20you%20can%20show%20or%20hide%20to%20other%20people%20who%20use%20Google%20services%3A)) and other less sensitive data about the user including coarse-grained buckets about the user (e.g. approximate location)
*   Ownership of a given **piece
[0m
[0m$ [0mcurl -s "https://crt.sh/?q=google.com&output=json" --max-time 120 | python3 -c "
import sys, json
try:
    raw = sys.stdin.read()
    if not raw.strip():
        print('Empty response')
        sys.exit(0)
    data = json.loads(raw)
    subdomains = set()
    for item in data:
        name = item.get('name_value', '')
        if name:
            for n in name.split('\n'):
                n = n.strip().lower()
                if n and 'google.com' in n:
                    subdomains.add(n)
    for s in sorted(subdomains):
        print(s)
except Exception as e:
    print(f'Error: {e}')
    print(f'Raw start: {raw[:200]}')
" | head -500
*.apis.corp.google.com
*.appengine.google.com
*.auth.corp.google.com
*.bigstore-test.corp.google.com
*.bigstore.corp.google.com
*.blogger.corp.google.com
*.blogspot.corp.google.com
*.c.docs.google.com
*.c.pack.google.com
*.c.play.google.com
*.c.video.google.com
*.cache1.c.docs.google.com
*.cache1.c.play.google.com
*.cache1.c.video.google.com
*.cache2.c.docs.google.com
*.cache2.c.play.google.com
*.cache2.c.video.google.com
*.cache3.c.docs.google.com
*.cache3.c.play.google.com
*.cache3.c.video.google.com
*.cache4.c.docs.google.com
*.cache4.c.play.google.com
*.cache4.c.video.google.com
*.cache5.c.docs.google.com
*.cache5.c.play.google.com
*.cache5.c.video.google.com
*.cache6.c.docs.google.com
*.cache6.c.play.google.com
*.cache6.c.video.google.com
*.cache7.c.docs.google.com
*.cache7.c.play.google.com
*.cache7.c.video.google.com
*.cache8.c.docs.google.com
*.cache8.c.play.google.com
*.cache8.c.video.google.com
*.cag.ext.google.com
*.chrome.google.com
*.client-channel.google.com
*.clients.google.com
*.cloud.google.com
*.code.google.com
*.corp-backups.corp.google.com
*.corp.google.com
*.dasher-qa.corp.google.com
*.dasher.corp.google.com
*.demetrius-codespot.corp.google.com
*.demetrius-googlecode.corp.google.com
*.demetrius.corp.google.com
*.devconsole-testers.sandbox.google.com
*.developer.google.com
*.developers.google.com
*.dfa7.corp.google.com
*.docs-dev.corp.google.com
*.docs-nightly.corp.google.com
*.docs-platinum.corp.google.com
*.docs-qa.corp.google.com
*.docs.google.com
*.docs.sandbox.google.com
*.drive-test.corp.google.com
*.drive.google.com
*.drive.sandbox.google.com
*.dthree.corp.google.com
*.ext.google.com
*.focus.corp.google.com
*.friendconnect.google.com
*.games.corp.google.com
*.git.corp.google.com
*.glass.ext.google.com
*.google.com
*.google.com.af
*.google.com.ag
*.google.com.ai
*.google.com.ar
*.google.com.au
*.google.com.bd
*.google.com.bh
*.google.com.bn
*.google.com.bo
*.google.com.br
*.google.com.by
*.google.com.bz
*.google.com.cn
*.google.com.co
*.google.com.cu
*.google.com.cy
*.google.com.do
*.google.com.ec
*.google.com.eg
*.google.com.et
*.google.com.fj
*.google.com.ge
*.google.com.gh
*.google.com.gi
*.google.com.gr
*.google.com.gt
*.google.com.hk
*.google.com.iq
*.google.com.jm
*.google.com.jo
*.google.com.kh
*.google.com.kw
*.google.com.lb
*.google.com.ly
*.google.com.mm
*.google.com.mt
*.google.com.mx
*.google.com.my
*.google.com.na
*.google.com.nf
*.google.com.ng
*.google.com.ni
*.google.com.np
*.google.com.nr
*.google.com.om
*.google.com.pa
*.google.com.pe
*.google.com.pg
*.google.com.ph
*.google.com.pk
*.google.com.pl
*.google.com.pr
*.google.com.py
*.google.com.qa
*.google.com.ru
*.google.com.sa
*.google.com.sb
*.google.com.sg
*.google.com.sl
*.google.com.sv
*.google.com.tj
*.google.com.tn
*.google.com.tr
*.google.com.tw
*.google.com.ua
*.google.com.uy
*.google.com.vc
*.google.com.ve
*.google.com.vn
*.googlesource.corp.google.com
*.ice.ext.google.com
*.jotspot-qa08.corp.google.com
*.loop.corp.google.com
*.mail.google.com
*.meeting.ext.google.com
*.orkut-fixprod.corp.google.com
*.orkut-impersonation.corp.google.com
*.orkut-ocdemo.corp.google.com
*.orkut-qa.corp.google.com
*.orkut-staging.corp.google.com
*.orkut-uberproxy.corp.google.com
*.orkut-vctask0.corp.google.com
*.orkut-vcvrfy.corp.google.com
*.orkut-yhtask0.corp.google.com
*.orkut-yhvrfy.corp.google.com
*.orkut-yqtask0.corp.google.com
*.orkut-yqvrfy.corp.google.com
*.oz-gmail.corp.google.com
*.oz-s2.corp.google.com
*.oz-www.corp.google.com
*.photos.google.com
*.plus.corp.google.com
*.plus.google.com
*.plusone.corp.google.com
*.postini.corp.google.com
*.profiles.corp.google.com
*.prom-qa.corp.google.com
*.prom-qa.sandbox.google.com
*.prom-test.corp.google.com
*.prom-test.sandbox.google.com
*.prom.corp.google.com
*.qa.adz.google.com
*.sandbox.google.com
*.sandbox.google.com.au
*.sandbox.google.com.br
*.sandbox.google.com.hk
*.script.sandbox.google.com
*.search.corp.google.com
*.sites-googlegroups-nightly.corp.google.com
*.sites-googlegroups-qa01.corp.google.com
*.sites-googlegroups-qa02.corp.google.com
*.sites-googlegroups-qa03.corp.google.com
*.sites-googlegroups-qa04.corp.google.com
*.sites-googlegroups-qa05.corp.google.com
*.sites-googlegroups-qa06.corp.google.com
*.sites-googlegroups-qa07.corp.google.com
*.sites-googlegroups-qa08.corp.google.com
*.sites-googlegroups-tctest.corp.google.com
*.sites.google.com
*.sites.sandbox.google.com
*.spdy-proxy.ext.google.com
*.staging-a.blogger.corp.google.com
*.staging-b.blogger.corp.google.com
*.staging-c.blogger.corp.google.com
*.staging-d.blogger.corp.google.com
*.staging-daily.blogger.corp.google.com
*.staging-daily.blogspot.corp.google.com
*.staging-gaia.blogger.corp.google.com
*.staging-git.corp.google.com
*.staging-googlesource.corp.google.com
*.staging-prod.blogger.corp.google.com
*.staging-weekly.blogger.corp.google.com
*.staging-weekly.blogspot.corp.google.com
*.talkgadget.google.com
*.test.postini.corp.google.com
*.upload.google.com
*.urchin.corp.google.com
*.url.google.com
*.vp.video.l.google.com
*.webdrive-test-canary.corp.google.com
*.webdrive-test-prod.corp.google.com
aarjav-b480g7k2ab9@checkout.google.com
accounts.flexpack.google.com
accounts.freezone.google.com
accounts.google.com
admin@google.com
ads-compare.eem.corp.google.com
adwords.google.com
adwords.google.com.ar
adwords.google.com.au
adwords.google.com.br
adwords.google.com.cn
adwords.google.com.gr
adwords.google.com.hk
adwords.google.com.ly
adwords.google.com.mx
adwords.google.com.my
adwords.google.com.pe
adwords.google.com.ph
adwords.google.com.pk
adwords.google.com.ru
adwords.google.com.sg
adwords.google.com.tr
adwords.google.com.tw
adwords.google.com.ua
adwords.google.com.vn
alt1.aspmx.l.google.com
alt1.gmail-smtp-in.l.google.com
alt1.gmr-smtp-in.l.google.com
alt2.aspmx.l.google.com
alt2.gmail-smtp-in.l.google.com
alt2.gmr-smtp-in.l.google.com
alt3.aspmx.l.google.com
alt3.gmail-smtp-in.l.google.com
alt3.gmr-smtp-in.l.google.com
alt4.aspmx.l.google.com
alt4.gmail-smtp-in.l.google.com
alt4.gmr-smtp-in.l.google.com
answers.google.com
apps-secure-data-connector.google.com
aspmx.l.google.com
audioads.google.com
bmcquade@google.com
cag.ext.google.com
cert-test.sandbox.google.com
checkout.google.com
cod.ext.google.com
da.ext.corp.google.com
da.ext.google.com
dg.video.google.com
ecc-test.sandbox.google.com
eggroll.ext.google.com
ext.google.com
flexpack.google.com
fra-da.ext.google.com
freezone.accounts.google.com
freezone.google.com
freezone.m.google.com
freezone.mail.google.com
gaiastaging.flexpack.google.com
gaiastaging.freezone.google.com
glass-eur.ext.google.com
glass-mtv.ext.google.com
glass-twd.ext.google.com
glass.ext.google.com
gmail-smtp-in.l.google.com
gmail.google.com
gmr-smtp-in.l.google.com
google.com
google.com.af
google.com.ag
google.com.ai
google.com.ar
google.com.au
google.com.bd
google.com.bh
google.com.bn
google.com.bo
google.com.br
google.com.by
google.com.bz
google.com.cn
google.com.co
google.com.cu
google.com.cy
google.com.do
google.com.ec
google.com.eg
google.com.et
google.com.fj
google.com.ge
google.com.gh
google.com.gi
google.com.gr
google.com.gt
google.com.hk
google.com.iq
google.com.jm
google.com.jo
google.com.kh
google.com.kw
google.com.lb
google.com.ly
google.com.mm
google.com.mt
google.com.mx
google.com.my
google.com.na
google.com.nf
google.com.ng
google.com.ni
google.com.np
google.com.nr
google.com.om
google.com.pa
google.com.pe
google.com.pg
google.com.ph
google.com.pk
google.com.pl
google.com.pr
google.com.py
google.com.qa
google.com.ru
google.com.sa
google.com.sb
google.com.sg
google.com.sl
google.com.sv
google.com.tj
google.com.tn
google.com.tr
google.com.tw
google.com.ua
google.com.uy
google.com.vc
google.com.ve
google.com.vn
hosted-id.google.com
hot-da.ext.google.com
hyd-da.ext.google.com
ice.ext.google.com
ics.prod.google.com
jmt0.google.com
login.corp.google.com
m.google.com
m.guts.corp.google.com
m.gutsdev.corp.google.com
mail.flexpack.google.com
mail.freezone.google.com
mail.google.com
meeting.ext.google.com
misc-sni.google.com
misc.google.com
mtalk.google.com
mtv-da-1.ad.corp.google.com
mtv-da.corp.google.com
mtv-da.ext.google.com
mx.google.com
mygeist.corp.google.com
mygeist2010.corp.google.com
news.freezone.google.com
onex.wifi.google.com
plus.flexpack.google.com
plus.freezone.google.com
proxyconfig.corp.google.com
qa.adz.google.com
reseed.corp.google.com
sandbox.google.com
search.flexpack.google.com
search.freezone.google.com
services.google.com
soaproxyprod01.ext.google.com
soaproxytest01.ext.google.com
spdy-proxy-debug.ext.google.com
spdy-proxy.ext.google.com
talk.google.com
twd-da.ext.google.com
twdsalesgsa.twd.corp.google.com
uberproxy-nocert.corp.google.com
uberproxy-san.corp.google.com
uberproxy.corp.google.com
upload.google.com
upload.video.google.com
vp.video.l.google.com
wifi.google.com
www.flexpack.google.com
www.freezone.google.com
www.google.com
www.google.com\341\205\232\341\205\232\341\205\232\341\205\232\341\205\232\341\205\232\341\205\232\341\205\232\341\205\232\341\205\232\341\205\232\341\205\232\341\205\232\341\205\232\341\205\232\341\205\232\341\205\232\341\205\232\341\205\232\341\205\232\341\205\232\341\205\232\341\205\232\341\205\232\341\205\232\341\205\232\341\205\232\341\205\232\341\205\232.phreedom.org
[0m
[0m$ [0mcurl -s "https://r.jina.ai/http://bughunters.google.com/about/rules/4849867320328192/cloud-vulnerability-reward-program-rules" | head -c 15000
Title: Cloud Vulnerability Reward Program Rules | Google Bug Hunters

URL Source: http://bughunters.google.com/about/rules/4849867320328192/cloud-vulnerability-reward-program-rules

Markdown Content:
The Google Cloud Vulnerability Reward Program recognizes the contributions of security researchers who invest their time and effort in helping us secure our platform and our customers. Through this program, we provide monetary rewards and public recognition for novel vulnerabilities disclosed to us.

## Services in scope [](https://bughunters.google.com/about/rules/google-friends/cloud-vulnerability-reward-program-rules#services-in-scope)

> 🎉 Wiz has joined Google Cloud! Vulnerability reports for Wiz are now welcome in the Cloud VRP. Note: Wiz is exempt from the standard 6-month blackout period for acquisition rewards. Happy hunting!

In principle, any [Google Cloud](https://cloud.google.com/products?e=48754805) product or web service that handles reasonably sensitive user data is intended to be in scope.

On the flip side, the program has several important exclusions to keep in mind:

*   **Google Workspace products** – Products belonging to [Google Workspace](https://workspace.google.com/) are out of scope for the Cloud VRP and should be reported to the [Google VRP](https://bughunters.google.com/about/rules/google-friends/6625378258649088/google-and-alphabet-vulnerability-reward-program-vrp-rules) instead.
*   **Third-party websites** – Some Google-branded services hosted in less common domains may be operated by our vendors or partners. We can't authorize you to test these systems on behalf of their owners and will not reward such reports. Please read the fine print on the page and examine domain and IP WHOIS records to confirm. If in doubt, talk to us first!
*   **Recent acquisitions** – To allow time for internal review and remediation, newly acquired companies are subject to a six-month blackout period. Bugs reported sooner than that will typically not qualify for a reward.

## Qualifying vulnerabilities [](https://bughunters.google.com/about/rules/google-friends/cloud-vulnerability-reward-program-rules#qualifying-vulnerabilities)

Any design or implementation issue that substantially affects the confidentiality or integrity of user data is likely to be in scope for the program. Common examples include:

*   Cross-site scripting,
*   Cross-site request forgery,
*   Mixed-content scripts,
*   Authentication or authorization flaws,
*   Server-side code execution bugs,
*   [XSLeak bugs](https://bughunters.google.com/learn/invalid-reports/web-platform/xsleaks/5022006283862016/xsleaks-and-xs-search).

Note that the scope of the program is limited to technical vulnerabilities in Google Cloud products and web services; please do not try to sneak into Google offices, attempt phishing attacks against our employees, and so on.

Out of concern for the availability of our services to all users, please do not attempt to carry out DoS attacks, leverage black hat SEO techniques, spam people, or do other similarly questionable things. We also discourage the use of any vulnerability testing tools that automatically generate very significant volumes of traffic.

## Non-qualifying vulnerabilities [](https://bughunters.google.com/about/rules/google-friends/cloud-vulnerability-reward-program-rules#non-qualifying-vulnerabilities)

**Google Cloud vulnerability reports where customer-owned resources were tested are ineligible for rewards.**

The Cloud VRP's scope is explicitly limited to Google-owned infrastructure and services. To protect our customer’s data, privacy, and operational integrity, testing of any customer resources within Google Cloud is strictly prohibited and will result in **ineligibility for reward**, regardless of any vulnerabilities discovered in Google infrastructure. Customer-owned resources include, but are not limited to, applications hosted on domains such as `*.bc.googleusercontent.com` or `*.appspot.com`. **Even if a researcher discovers a vulnerability in Google-owned infrastructure, if the discovery process involved testing or accessing customer resources, the report will be deemed ineligible for a reward.**

Specifically, researchers are NOT authorized to:

*   Perform any form of penetration testing, vulnerability scanning, or security assessment against customer-owned instances, applications, or data within Google Cloud without the express consent of that specific customer.
    *   Broad scanning across large, unsegmented IP blocks is not permitted as part of the Google Cloud VRP. For example, a researcher should NOT scan through a block of IP addresses known to be primarily used by Google Cloud customers, even if they end up identifying a Google-owned virtual machine within that range. This is considered testing within customer space.

*   Access, manipulate, or extract data from customer resources, even if unintentional or required to discover a potential Google-owned vulnerability.

**Authorized Testing Scope:** Researchers are permitted to conduct targeted testing starting from the beginning of a [known Google-owned IP range](https://support.google.com/a/answer/10026322?hl=en). However, any testing activity that requires or involves accessing or interacting with customer resources is strictly outside the scope of this program. Alternatively, researchers should set up their own Cloud projects and resources to find and test vulnerabilities.

**Important Reminder:** Google Cloud customers may independently authorize penetration testing of their own applications. However, such authorizations do not extend to the Cloud VRP, and testing customer resources without explicit customer authorization is not permitted.

**Identifying Prohibited IP Addresses:**

*   Note the [IP ranges reserved for Google Cloud customers](https://www.gstatic.com/ipranges/cloud.json).
*   Run `whois` against a target IP address. The following response indicates customer-owned resources: `*** The IP addresses under this Org-ID are in use by Google Cloud customers ***`.
*   Run `nslookup` against a target IP address. The following responses indicate customer-owned resources: `*.bc.googleusercontent.com` or `*.appspot.com`

### Vulnerabilities within Third-Party Software Components [](https://bughunters.google.com/about/rules/google-friends/cloud-vulnerability-reward-program-rules#vulnerabilities-within-third-party-software-components)

Reports on vulnerabilities where the root cause is within the code of a third-party product that Google Cloud offers as a managed service (e.g., a flaw in the core Microsoft SQL Server engine itself, discovered while using Cloud SQL) are handled as follows:

*   **High Severity Impact (S0):** If the vulnerability is assessed by Google as S0 severity **and** it demonstrably impacts the security of Google Cloud customers, it may qualify for a reward. Eligible reports will be rewarded based on the assessed impact and severity according to the standard [Reward amounts](https://bughunters.google.com/about/rules/google-friends/cloud-vulnerability-reward-program-rules#reward-amounts) table, commensurate with the impact on the Google Cloud service.
*   **Lower Severity Impact (S1 or lower):** Vulnerabilities within the third-party product code assessed as S1 severity or lower are not eligible for rewards from the Google Cloud VRP.
*   **Reporting to the Vendor:** Regardless of the severity, we encourage researchers to also disclose the vulnerability directly to the third-party vendor. Google is committed to [responsible disclosure](https://about.google/company-info/appsecurity/) and will notify the vendor of such vulnerabilities reported to us.
*   **Vulnerabilities in Google's implementation**, integration, configuration, or management layer _around_ the third-party service remain in scope as per the standard Cloud VRP rules.

### Other non-qualifying findings [](https://bughunters.google.com/about/rules/google-friends/cloud-vulnerability-reward-program-rules#other-non-qualifying-findings)

**Note:** Visit our [Bug Hunter University](https://bughunters.google.com/learn) page dedicated to common non-qualifying findings and vulnerabilities.

The main reason why a reported issue may not qualify for a reward is because of low impact. Although we review each report on a case-by-case basis, we are sharing some of the common low-risk issues that typically do not earn a monetary reward:

*   **Cross-site scripting vulnerabilities in “sandbox” domains** ([_read more_](https://bughunters.google.com/learn/invalid-reports/web-platform/xss/6619189462433792).) We maintain a number of domains that leverage the same-origin policy to safely isolate certain types of untrusted content; the most prominent example of this is "*.googleusercontent.com". Unless an impact on sensitive user data can be demonstrated, we do not consider the ability to execute JavaScript in that domain to be a bug.
*   **URL redirection** ([_read more_](https://bughunters.google.com/learn/invalid-reports/navigation/6680364896223232).) We recognize that the address bar is the only reliable security indicator in modern browsers; consequently, we hold that the usability and security benefits of a small number of well-designed and closely monitored redirectors outweigh their true risks.
*   **Legitimate content proxying and framing.** We expect our services to unambiguously label third-party content and to perform a number of abuse-detection checks, but as with redirectors, we think that the value the risk.
*   **Bugs requiring exceedingly unlikely user interaction.** For example, a cross-site scripting flaw that requires the victim to manually type in an XSS payload and then double-click an error message may realistically not meet the bar.
*   **Logout cross-site request forgery** ([_read more_](https://bughunters.google.com/learn/invalid-reports/web-platform/csrf-clickjacking/5072689380982784).) For better or worse, the design of HTTP cookies means that no single website can prevent its users from being logged out; consequently, application-specific ways of achieving this goal will likely not qualify. You may be interested in personal blog posts from [Chris Evans](http://scarybeastsecurity.blogspot.com/2010/01/logout-xsrf-significant-web-app-bug.html) and [Michal Zalewski](http://lcamtuf.blogspot.com/2010/10/http-cookies-or-how-not-to-design.html) for more background.
*   **Flaws affecting the users of out-of-date browsers and plugins.** The security model of the web is constantly being fine-tuned. The panel typically does not reward reports that describe issues that affect only the users of outdated or unpatched browsers.
*   **Presence of banner or version information.** Version information does not, by itself, expose the service to attacks - so we do not consider this to be a bug. That said, if you find outdated software and have good reasons to suspect that it poses a well-defined security risk, please let us know.
*   **User enumeration.** Reports outlining user enumeration are not within scope unless you can demonstrate that we don't have any rate limits in place to protect our users.
*   **Bypassing the limit of accounts that can be verified with a given SMS number.** We often receive reports about users being able to bypass our SMS limit for verifying accounts. There are actually two different quotas per number for account verification, one via 'SMS' and a different one via 'Call Me'.
*   **Activity within the scope of your own provisioned resources.** If you have root/admin in your own isolated instance (like Cloud Shell, GCE, etc.), things you do inside that instance aren't a vulnerability unless you can affect other users' or Google's infra. This includes looking at old data on your disk.
*   **No PoC**: Reports that lack a functional proof-of-concept.
*   **Customer Misconfiguration / Shared Responsibility**: Insecure customer configurations (such as unconditionally injecting shared secrets or misconfiguring security-related settings) rather than a product vulnerability. This also includes vulnerabilities that exist entirely within the application code deployed by the customer.
*   **UI discrepancy**: In some cases the underlying APIs may have a different behaviour than the (web) UI on top of it. We usually consider the API to be the source of truth. Reports focusing on discrepancies between the UI and the API usually don't receive a monetary reward, unless the story around it clearly demonstrates the bypass of a security boundary.
*   **Social Engineering Pre-requisite**: Theoretical attacks requiring victims to execute double-click scenarios on previously-installed malicious software or inserting arbitrary authorized domain strings for Google One Tap prompts are generally rejected; these vectors operate externally via social engineering rather than indicating platform vulnerabilities.

Monetary rewards aside, vulnerability reporters who work with us to resolve security bugs in our products will be credited on the [Leaderboard](https://bughunters.google.com/leaderboard). If we file an internal security bug, we will acknowledge your contribution on that page.

## Reward amounts [](https://bughunters.google.com/about/rules/google-friends/cloud-vulnerability-reward-program-rules#reward-amounts)

The following table outlines typical rewards for the most common classes of bugs.

**Applicable to reports submitted on or after October 1, 2025**

| Severity Impact Category / Impact Tier | Description | Google Cloud products on [Tier 1](https://github.com/google/bughunters/blob/main/cloud_tiers/cloud-tiers.asciipb)**(IT1)** | Google Cloud products on [Tier 2](https://github.com/google/bughunters/blob/main/cloud_tiers/cloud-tiers.asciipb)**(IT2)** | Default Google Cloud products **(IT3)** | Acquired Google Cloud products **(IT3a)** | Other acquisitions or lower priority Google Cloud products **(IT3b, Generic)** |
| --- | --- | --- | --- | --- | --- | --- |
| **Vulnerabilities without any interaction or relationship between attacker and victim, and the impact must involve unauthorized access to sensitive data or actions (e.g., metadata access may not qualify).** |
| (S0a) Compromise of Google Cloud Production Environment | Vulnerabilities that allow an attacker to breach the security boundaries of the underlying Google Infrastructure that runs Cloud services. | $50,000 - $101,010 | $2,500 - $20,000 |
| (S0b) Cloud Project/Organization Takeover with Full Administrative Control | Gaining the highest level of control over a project, equivalent to `roles/owner`. This includes the ability to set any IAM policy on any resource, `iam.setIamPolicy`, manage all service accounts, and control all resources within that project. | $75,000 | $50,000 | $31,337 | $25,000 | $20,000 |
| (S0c) Multi-Service Privilege Escalation - WRITE | Ability to gain modification capabilities on resources in multiple GCP services within the customer project, or to obta
[0m
[0m$ [0mcurl -s "https://r.jina.ai/http://bughunters.google.com/about/rules/5222232590712832/ai-vulnerability-reward-program-rules" | head -c 15000
Title: AI Vulnerability Reward Program Rules | Google Bug Hunters

URL Source: http://bughunters.google.com/about/rules/5222232590712832/ai-vulnerability-reward-program-rules

Markdown Content:
In October 2023, Google announced initial reward criteria for reporting bugs in AI products. In October 2025, we're expanding and clarifying our AI rewards program with the launch of this AI Vulnerability Reward Program. This program allows us to reward security researchers who invest their time and effort to discover and report AI-related vulnerabilities, assisting us in securing our platforms and our users.

## Scope [](https://bughunters.google.com/about/rules/google-friends/ai-vulnerability-reward-program-rules#scope)

The AI Vulnerability Rewards Program (VRP) covers AI-related vulnerability and abuse issues in Google and Alphabet AI products. See the [Rewards](https://bughunters.google.com/about/rules/5222232590712832/ai-vulnerability-reward-program-rules#rewards) section below for detail. AI-related issues are those issues where interaction with a Large Language Model (LLM) or other Generative AI (GenAI) system, such as a natural language interaction, is an integral part of the vulnerability or abuse issue.

**Note: Except where [otherwise noted](https://bughunters.google.com/about/rules/5222232590712832/ai-vulnerability-reward-program-rules#ai-vrp-product-tiers), issues found in Vertex AI or other [Google Cloud products](https://cloud.google.com/products?e=48754805) are covered by the [Google Cloud Vulnerability Rewards Program](https://bughunters.google.com/about/rules/google-friends/4849867320328192/cloud-vulnerability-reward-program-rules#services-in-scope), and are out of scope for this AI VRP.**

## Ineligible report types - Read this before you submit! [](https://bughunters.google.com/about/rules/google-friends/ai-vulnerability-reward-program-rules#ineligible-report-types-read-this-before-you-submit-)

When submitting AI vulnerability reports, take note that [non-qualifying issues and vulnerabilities](https://bughunters.google.com/about/rules/5222232590712832/ai-vulnerability-reward-program-rules#non-qualifying-vulnerabilities) are ineligible for reward. Reports that fall in these categories will not be accepted, and your report will be closed. Examples include:

*   Generating violative, misleading, or factually incorrect content within the attacker's own session (including standard "jailbreaks" and "hallucinations"). Please report inappropriate content using in-product links.
*   AI-generated content-based issues, including reports of AI safety or alignment bypasses. Please report this content using in-product links.
*   Compliance, legal, or intellectual property issues, including issues relating to country/region-specific laws such as privacy or intellectual property laws. Please report those issues to the [Report Content for Legal Reasons](https://support.google.com/legal/answer/3110420) process.
*   Preamble extraction without sensitive information leakage.
*   Execution of arbitrary code within a sandboxed environment (Gemini provides access to code execution tools).
*   Contexts in which a model's incorrect output or classification does not pose a compelling attack scenario or feasible path to user harm.
*   Insecure or unsafe MCP server interaction, when configured by the victim.

Reports must be verified by the reporter and demonstrate a clear in-scope threat, risk, or vulnerability in plain language. Please carefully review the [list of non-qualifying vulnerabilities](https://bughunters.google.com/about/rules/5222232590712832/ai-vulnerability-reward-program-rules#non-qualifying-vulnerabilities) before filing your report.

## Qualifying vulnerabilities [](https://bughunters.google.com/about/rules/google-friends/ai-vulnerability-reward-program-rules#qualifying-vulnerabilities)

The following table details our criteria for AI bug reports to assist our bug hunting community in effectively testing the safety and security of our AI products. Our scope aims to facilitate testing for traditional security vulnerabilities as well as risks specific to AI systems.

| Category | Example Attack Scenario |
| --- | --- |
| **S1: Rogue Actions** Attacks that modify the state of the victim’s account or data with a clear security impact[1]. | Indirect prompt injection allows an attacker to unexpectedly, and without confirmation, cause Google Home to take an action, such as unlocking a door. |
| **S2: Sensitive Data Exfiltration** Attacks that leak victim’s [SPII](https://developers.google.com/standard-payments/reference/glossary#spii), [PII](https://developers.google.com/standard-payments/reference/glossary#pii), or other sensitive data without an effective opportunity for user approval. | Indirect prompt injection allows an attacker to summarize all of a victim's email, and send the summary to an attacker-controlled account. |
| **A1: Phishing Enablement** Persistent, cross-user HTML injection on a Google-branded site which: (a) does not include a “user-generated content” warning, and (b) at the panel’s discretion, presents a convincing phishing attack vector. **Note**: Sites intended for user-generated content are excluded from this category. | An AI product allows an attacker to share an attacker-generated web page, hiding the user-generated content warning. The attacker can distribute this page, and a user would be reasonably convinced that the page is a legitimate Google site. |
| **A2: Model Theft** Attacks that exfiltrate complete, detailed, and confidential model parameters. **Note**: System prompts and training corpora are almost always non-confidential and specifically out of scope. **Note**: Successful reports in this category are extremely rare. | A Google-exposed API allows unintentional exfiltration of exact, detailed loss values for arbitrary inputs from a Google-proprietary, confidential model. |
| **A3: Context Manipulation (Cross-account)** Attacks that allow for repeatable, persistent manipulation of the context of a victim’s AI environment, that is hidden from the victim and does not require significant victim interaction. **Note**: Context manipulation that results in rogue action or sensitive data exfiltration will be rewarded under those rules respectively. | An attacker is able to send a calendar invite to a victim, causing a memory to be stored in an AI product; the product takes unconfirmed, but non-security-sensitive, future actions based on that stored memory. |
| **A4: Access Control Bypass (Limited security impact)** Attacks that allow a user to exfiltrate data which is otherwise inaccessible. **Note**: Exfiltration of security-sensitive content, at the panel’s discretion, would be covered by “Sensitive Data Exfiltration” above. | An AI product is able to provide content from an otherwise inaccessible document not containing user data, such as a Google’s campus lunch menus, where the attack does not rise to the level of a serious security incident. |
| **A5: Unauthorized Product Usage** Attacks that allow Google server-side features to be enabled on the user’s account without authorization or billing. **Note**: The enabled feature must be functional, not merely visible. | An attacker is able to modify parameters on a signup form to enable and use an AI feature that has not been publicly announced. |
| **A6: Cross-user Denial of Service (with caveats!)** Attacks that cause persistent denial of service for a feature or product in a (non-attacker) victim account with a limited set of requests. | Joining a public event or forum with a malformed user profile causes product crashes for other users, where the attacker account is not specifically invited or visible to victim users. **Note**: Per our [Code of Conduct](https://bughunters.google.com/about/rules/other/6009584292331520/code-of-conduct-for-our-vulnerability-reward-programs#inappropriate-testing-and-data-handling), your testing must not disrupt any account you don’t own. Volumetric DoS attacks are specifically prohibited. The ability to DoS your current account is excluded from this category. |
| **Other Abuse or Security Issues** If you find a flaw in an AI-powered tool other than what is listed above, you can still submit, provided that it meets the qualifications listed on this page. | A bug or behavior that clearly meets our qualifications for a valid security or abuse issue. |

[1] For example, a bug which can create an email draft with arbitrary content does modify the state of a victim's account, but it does not clearly demonstrate an impact of the attack.

## Non-qualifying vulnerabilities [](https://bughunters.google.com/about/rules/google-friends/ai-vulnerability-reward-program-rules#non-qualifying-vulnerabilities)

In addition to the list of [non-qualifying vulnerabilities for the Google VRP](https://bughunters.google.com/about/rules/google-friends/6625378258649088/google-and-alphabet-vulnerability-reward-program-vrp-rules#non-qualifying-vulnerabilities), please review the table below detailing the explicit, non-qualifying vulnerabilities for Prompt Injection techniques, which should be reported via in-product feedback channels instead of the AI VRP.

| Common Outcome / Technique | Description / Examples |
| --- | --- |
| Policy-Violating Content Generation | Generating violative content (such as hate speech, violent content, or illegal instructions), misleading information, or factually incorrect/fabricated content within the user's own session (e.g. jailbreaks, safety bypasses, or alignment bypasses). |
| Guardrail Bypass | Circumventing content filters or instructions (e.g., using obfuscation, translation, role-playing) to achieve policy-violating content generation (see above). |
| Harm Achievable Through Existing Non-AI Tools | Using an AI product to do something potentially harmful that is already possible with other tools. For example, finding a vulnerability in open source software (already possible using publicly available [static analysis tools](https://en.wikipedia.org/wiki/Static_application_security_testing)), or producing the answer to a harmful question when the answer is otherwise available online. |
| Hallucinations / Factual Inaccuracy | Model generates incorrect, nonsensical, or fabricated information, including fake code execution results, APIs, or credentials. |
| No Compelling Harm from Incorrect Output / Classification | Contexts in which a model's incorrect output or classification does not pose a compelling attack scenario or feasible path to user harm. |
| Intended Functionality | Demonstrating expected behavior, e.g., accessing / summarizing the user's own data when permitted. |
| Preamble / Prompt Extraction (Non-Sensitive) | Extracting system prompts or preambles that do not contain sensitive, non-public information. |
| Extraction of Non-Sensitive / Public Information | Extraction of non-sensitive / public model information (e.g., model name or architectural approach, if publicly known). |
| Imprecise / Non-confidential Model Weight Extraction | Attacks in which the architecture and weights are extracted without precision, or weights are extracted from a non-confidential model (e.g. Gemma). |
| Low-Impact Disruptions to User Experience | Minor disruptions like adding harmless text, minor formatting errors, easily ignored deviations from a user’s typical experience. |
| Intellectual Property Issues | Content which appears to violate copyright or other intellectual property rights. Google's generative AI products already include a dedicated reporting channel for these types of content issues, accessible via in-product links. |

Google's generative AI products already have a dedicated reporting channel for content-based issues. You can also report inappropriate content for legal reasons [here](https://support.google.com/legal/answer/3110420).

## Rewards [](https://bughunters.google.com/about/rules/google-friends/ai-vulnerability-reward-program-rules#rewards)

AI VRP rewards amounts are determined based on the product impacted by the issue ("AI VRP Product Tiers"), and the Category of the issue, as shown in the table below.

### AI VRP Product Tiers [](https://bughunters.google.com/about/rules/google-friends/ai-vulnerability-reward-program-rules#ai-vrp-product-tiers)

#### Flagship [](https://bughunters.google.com/about/rules/google-friends/ai-vulnerability-reward-program-rules#flagship)

AI features in Flagship Google AI products, specifically:

*   [Google Search](https://google.com/) (on google.com),
*   [Gemini Apps](https://support.google.com/gemini/answer/13275745) ([Web](https://gemini.google.com/), [Android](https://play.google.com/store/apps/details?id=com.google.android.apps.bard), and [iOS](https://apps.apple.com/us/app/google-gemini/id6477489729)), and
*   [Google Workspace](https://workspace.google.com/) core applications (i.e., Gmail, Drive, Meet, Calendar, Docs, Sheets, Slides, and Forms).

#### Standard [](https://bughunters.google.com/about/rules/google-friends/ai-vulnerability-reward-program-rules#standard)

AI features in high-sensitivity Google AI products, including:

*   [AI Studio](https://aistudio.google.com/),
*   [Jules](https://jules.google.com/), and
*   [Google Workspace](https://workspace.google.com/) non-core applications (i.e., NotebookLM, Appsheet, etc).

#### Other [](https://bughunters.google.com/about/rules/google-friends/ai-vulnerability-reward-program-rules#other)

Other AI integrations in Google products, **excluding**:

*   [Tier3 or Tier4 acquisitions](https://github.com/google/bughunters/blob/main/domain-tiers/external_domains_acquisitions.asciipb),
*   [other Tier 3 or Tier 4 applications](https://github.com/google/bughunters/blob/main/domain-tiers/external_domains_google.asciipb), and
*   Other open-source, non-proprietary, or third-party applications.

### Reward amounts [](https://bughunters.google.com/about/rules/google-friends/ai-vulnerability-reward-program-rules#reward-amounts)

The following table outlines the standard rewards for the most common classes of bugs, and the sections that follow it describe how these rewards can be adjusted to take into account characteristics such as report quality and other factors.

| Category / VRP Product Tier | Flagship | Standard | Other |
| --- | --- | --- | --- |
| **S1: Rogue Actions** | $20,000 | $15,000 | $10,000 |
| **S2: Sensitive Data Exfiltration** | $15,000 | $15,000 | $10,000 |
| **A1: Phishing Enablement** | $5,000 | $500 | credit |
| **A2: Model Theft** | $5,000 | $500 | credit |
| **A3: Context Manipulation** | $5,000 | $500 | credit |
| **A4: Access Control Bypass** | $2,500 | $250 | credit |
| **A5: Unauthorized Product Usage** | $1,000 | $100 | credit |
| **A6: Cross-user Denial of Service** | $500 | $100 | credit |

The final amount is always chosen at the discretion of the reward panel. We understand that some of you are not interested in money. We offer the option
[0m
[0m$ [0mcurl -s "https://raw.githubusercontent.com/google/bughunters/main/domain-tiers/external_domains_google.asciipb" | head -c 10000
domain: {
  fqdn: "flash.android.com"
  tier: TIER0
}
domain: {
  fqdn: "accounts.google.com"
  tier: TIER0
}
domain: {
  fqdn: "console.actions.google.com"
  tier: TIER0
}
domain: {
  fqdn: "admin.google.com"
  tier: TIER0
}
domain: {
  fqdn: "bard.google.com"
  tier: TIER0
}
domain: {
  fqdn: "chrome.google.com"
  tier: TIER0
}
domain: {
  fqdn: "chromewebstore.google.com"
  tier: TIER0
}
domain: {
  fqdn: "clients6.google.com"
  tier: TIER0
}
domain: {
  fqdn: "accounts.cloud.google.com"
  tier: TIER0
}
domain: {
  fqdn: "console.cloud.google.com"
  tier: TIER0
}
domain: {
  fqdn: "console-secure.cloud.google.com"
  tier: TIER0
}
domain: {
  fqdn: "ide.cloud.google.com"
  tier: TIER0
}
domain: {
  fqdn: "shell.cloud.google.com"
  tier: TIER0
}
domain: {
  fqdn: "ssh.cloud.google.com"
  tier: TIER0
}
domain: {
  fqdn: "ssh-secure.cloud.google.com"
  tier: TIER0
}
domain: {
  fqdn: "cloudsearch.google.com"
  tier: TIER0
}
domain: {
  fqdn: "cloudssh.developers.google.com"
  tier: TIER0
}
domain: {
  fqdn: "console.developers.google.com"
  tier: TIER0
}
domain: {
  fqdn: "ediscovery.google.com"
  tier: TIER0
}
domain: {
  fqdn: "fi.google.com"
  tier: TIER0
}
domain: {
  fqdn: "console.firebase.google.com"
  tier: TIER0
}
domain: {
  fqdn: "gemini.google.com"
  tier: TIER0
}
domain: {
  fqdn: "inbox.google.com"
  tier: TIER0
}
domain: {
  fqdn: "issuetracker.google.com"
  tier: TIER0
}
domain: {
  fqdn: "mail.google.com"
  tier: TIER0
}
domain: {
  fqdn: "mail-settings.google.com"
  tier: TIER0
}
domain: {
  fqdn: "meet.google.com"
  tier: TIER0
}
domain: {
  fqdn: "myaccount.google.com"
  tier: TIER0
}
domain: {
  fqdn: "passwords.google.com"
  tier: TIER0
}
domain: {
  fqdn: "play.google.com"
  tier: TIER0
}
domain: {
  fqdn: "remotedesktop.google.com"
  tier: TIER0
}
domain: {
  fqdn: "takeout.google.com"
  tier: TIER0
}
domain: {
  fqdn: "vault.google.com"
  tier: TIER0
}
domain: {
  fqdn: "googlecloudcheatsheet.withgoogle.com"
  tier: TIER0
}
domain: {
  fqdn: "console.cloud.google"
  tier: TIER0
}
domain: {
  fqdn: "bugs.chromium.org"
  tier: TIER0
}
domain: {
  fqdn: "g-issues.chromium.org"
  tier: TIER0
}
domain {
  fqdn: "bughunters.google.com"
  tier: TIER0
}
domain: {
  fqdn: "issues.chromium.org"
  tier: TIER0
}
domain: {
  fqdn: "enexpress.app"
  tier: TIER1
}
domain: {
  fqdn: "threadit.app"
  tier: TIER1
}
domain: {
  fqdn: "ci.android.com"
  tier: TIER1
}
domain: {
  fqdn: "orionwifi.area120.com"
  tier: TIER1
}
domain: {
  fqdn: "signals.area120.com"
  tier: TIER1
}
domain: {
  fqdn: "threadit.area120.com"
  tier: TIER1
}
domain: {
  fqdn: "ads.google.com"
  tier: TIER1
}
domain: {
  fqdn: "adsense.google.com"
  tier: TIER1
}
domain: {
  fqdn: "adwords.google.com"
  tier: TIER1
}
domain: {
  fqdn: "android.google.com"
  tier: TIER1
}
domain: {
  fqdn: "baseline.google.com"
  tier: TIER1
}
domain: {
  fqdn: "admin.baseline.google.com"
  tier: TIER1
}
domain: {
  fqdn: "businessmessages.google.com"
  tier: TIER1
}
domain: {
  fqdn: "calendar.google.com"
  tier: TIER1
}
domain: {
  fqdn: "chat.google.com"
  tier: TIER1
}
domain: {
  fqdn: "clients.google.com"
  tier: TIER1
}
domain: {
  fqdn: "clients1.google.com"
  tier: TIER1
}
domain: {
  fqdn: "clients2.google.com"
  tier: TIER1
}
domain: {
  fqdn: "clients3.google.com"
  tier: TIER1
}
domain: {
  fqdn: "clients4.google.com"
  tier: TIER1
}
domain: {
  fqdn: "clients5.google.com"
  tier: TIER1
}
domain: {
  fqdn: "bigquery.cloud.google.com"
  tier: TIER1
}
domain: {
  fqdn: "channelservices.cloud.google.com"
  tier: TIER1
}
domain: {
  fqdn: "partners.cloud.google.com"
  tier: TIER1
}
domain: {
  fqdn: "source.cloud.google.com"
  tier: TIER1
}
domain: {
  fqdn: "storage.cloud.google.com"
  tier: TIER1
}
domain: {
  fqdn: "docs.google.com"
  tier: TIER1
}
domain: {
  fqdn: "domains.google.com"
  tier: TIER1
}
domain: {
  fqdn: "drive.google.com"
  tier: TIER1
}
domain: {
  fqdn: "encrypted.google.com"
  tier: TIER1
}
domain: {
  fqdn: "express.google.com"
  tier: TIER1
}
domain: {
  fqdn: "get.google.com"
  tier: TIER1
}
domain: {
  fqdn: "groups.google.com"
  tier: TIER1
}
domain: {
  fqdn: "hangouts.google.com"
  tier: TIER1
}
domain: {
  fqdn: "home.google.com"
  tier: TIER1
}
domain: {
  fqdn: "hume.google.com"
  tier: TIER1
}
domain: {
  fqdn: "ipv6.google.com"
  tier: TIER1
}
domain: {
  fqdn: "lens.google.com"
  tier: TIER1
}
domain: {
  fqdn: "lers.google.com"
  tier: TIER1
}
domain: {
  fqdn: "messages.google.com"
  tier: TIER1
}
domain: {
  fqdn: "myactivity.google.com"
  tier: TIER1
}
domain: {
  fqdn: "notifications.google.com"
  tier: TIER1
}
domain: {
  fqdn: "pay.google.com"
  tier: TIER1
}
domain: {
  fqdn: "payments.google.com"
  tier: TIER1
}
domain: {
  fqdn: "photos.google.com"
  tier: TIER1
}
domain: {
  fqdn: "admin-baseline.sandbox.google.com"
  tier: TIER1
}
domain: {
  fqdn: "baseline.sandbox.google.com"
  tier: TIER1
}
domain: {
  fqdn: "script.google.com"
  tier: TIER1
}
domain: {
  fqdn: "shop.google.com"
  tier: TIER1
}
domain: {
  fqdn: "shopping.google.com"
  tier: TIER1
}
domain: {
  fqdn: "store.google.com"
  tier: TIER1
}
domain: {
  fqdn: "talkgadget.google.com"
  tier: TIER1
}
domain: {
  fqdn: "timeline.google.com"
  tier: TIER1
}
domain: {
  fqdn: "timeline-autopush.google.com"
  tier: TIER1
}
domain: {
  fqdn: "timeline-staging.google.com"
  tier: TIER1
}
domain: {
  fqdn: "voice.google.com"
  tier: TIER1
}
domain: {
  fqdn: "wallet.google.com"
  tier: TIER1
}
domain: {
  fqdn: "www.google.com"
  tier: TIER1
}
domain: {
  fqdn: "toolbox.googleapps.com"
  tier: TIER1
}
domain: {
  fqdn: "googlesource.com"
  tier: TIER1
}
domain: {
  fqdn: "checkout.youtube.com"
  tier: TIER1
}
domain: {
  fqdn: "m.youtube.com"
  tier: TIER1
}
domain: {
  fqdn: "payments.youtube.com"
  tier: TIER1
}
domain: {
  fqdn: "studio.youtube.com"
  tier: TIER1
}
domain: {
  fqdn: "www.youtube.com"
  tier: TIER1
}
domain: {
  fqdn: "legalretrievals.google"
  tier: TIER1
}
domain: {
  fqdn: "orion.google"
  tier: TIER1
}
domain: {
  fqdn: "wiz.io"
  tier: TIER1
}
domain: {
  fqdn: "grasshopper.app"
  tier: TIER2
}
domain: {
  fqdn: "kormo.app"
  tier: TIER2
}
domain: {
  fqdn: "admob.com"
  tier: TIER2
}
domain: {
  fqdn: "partner.android.com"
  tier: TIER2
}
domain: {
  fqdn: "blogger.com"
  tier: TIER2
}
domain: {
  fqdn: "gfiber.com"
  tier: TIER2
}
domain: {
  fqdn: "google.com"
  tier: TIER2
}
domain: {
  fqdn: "360suite.google.com"
  tier: TIER2
}
domain: {
  fqdn: "aboutme.google.com"
  tier: TIER2
}
domain: {
  fqdn: "adsdatahub.google.com"
  tier: TIER2
}
domain: {
  fqdn: "adservice.google.com"
  tier: TIER2
}
domain: {
  fqdn: "adssettings.google.com"
  tier: TIER2
}
domain: {
  fqdn: "adviser.google.com"
  tier: TIER2
}
domain: {
  fqdn: "analytics.google.com"
  tier: TIER2
}
domain: {
  fqdn: "apis.google.com"
  tier: TIER2
}
domain: {
  fqdn: "appengine.google.com"
  tier: TIER2
}
domain: {
  fqdn: "appmaker.google.com"
  tier: TIER2
}
domain: {
  fqdn: "area120.google.com"
  tier: TIER2
}
domain: {
  fqdn: "artsandculture.google.com"
  tier: TIER2
}
domain: {
  fqdn: "audiencecenter.google.com"
  tier: TIER2
}
domain: {
  fqdn: "automotiveservices.google.com"
  tier: TIER2
}
domain: {
  fqdn: "books.google.com"
  tier: TIER2
}
domain: {
  fqdn: "campaignmanager.google.com"
  tier: TIER2
}
domain: {
  fqdn: "careers.google.com"
  tier: TIER2
}
domain: {
  fqdn: "classroom.google.com"
  tier: TIER2
}
domain: {
  fqdn: "cloud.google.com"
  tier: TIER2
}
domain: {
  fqdn: "sdk.cloud.google.com"
  tier: TIER2
}
domain: {
  fqdn: "status.cloud.google.com"
  tier: TIER2
}
domain: {
  fqdn: "contributor.google.com"
  tier: TIER2
}
domain: {
  fqdn: "usercontent.corp.google.com"
  tier: TIER2
}
domain: {
  fqdn: "crowdsource.google.com"
  tier: TIER2
}
domain: {
  fqdn: "cse.google.com"
  tier: TIER2
}
domain: {
  fqdn: "datastudio.google.com"
  tier: TIER2
}
domain: {
  fqdn: "ddm.google.com"
  tier: TIER2
}
domain: {
  fqdn: "developers.google.com"
  tier: TIER2
}
domain: {
  fqdn: "apps.developers.google.com"
  tier: TIER2
}
domain: {
  fqdn: "cla.developers.google.com"
  tier: TIER2
}
domain: {
  fqdn: "codelabs.developers.google.com"
  tier: TIER2
}
domain: {
  fqdn: "payments.developers.google.com"
  tier: TIER2
}
domain: {
  fqdn: "dns.google.com"
  tier: TIER2
}
domain: {
  fqdn: "donate.google.com"
  tier: TIER2
}
domain: {
  fqdn: "doubleclick.google.com"
  tier: TIER2
}
domain: {
  fqdn: "earth.google.com"
  tier: TIER2
}
domain: {
  fqdn: "earthengine.google.com"
  tier: TIER2
}
domain: {
  fqdn: "code.earthengine.google.com"
  tier: TIER2
}
domain: {
  fqdn: "explorer.earthengine.google.com"
  tier: TIER2
}
domain: {
  fqdn: "signup.earthengine.google.com"
  tier: TIER2
}
domain: {
  fqdn: "families.google.com"
  tier: TIER2
}
domain: {
  fqdn: "fiber.google.com"
  tier: TIER2
}
domain: {
  fqdn: "console.fiber.google.com"
  tier: TIER2
}
domain: {
  fqdn: "partnerportal.fiber.google.com"
  tier: TIER2
}
domain: {
  fqdn: "toolbox.fiber.google.com"
  tier: TIER2
}
domain: {
  fqdn: "firebase.google.com"
  tier: TIER2
}
domain: {
  fqdn: "fit.google.com"
  tier: TIER2
}
domain: {
  fqdn: "flights.google.com"
  tier: TIER2
}
domain: {
  fqdn: "fonts.google.com"
  tier: TIER2
}
domain: {
  fqdn: "fusiontables.google.com"
  tier: TIER2
}
domain: {
  fqdn: "gsuite.google.com"
  tier: TIER2
}
domain: {
  fqdn: "health.google.com"
  tier: TIER2
}
domain: {
  fqdn: "images.google.com"
  tier: TIER2
}
domain: {
  fqdn: "inputtools.google.com"
  tier: TIER2
}
domain: {
  fqdn: "ipv6test.google.com"
  tier: TIER2
}
domain: {
  fqdn: "isp.google.com"
  tier: TIER2
}
domain: {
  fqdn: "jamboard.google.com"
  tier: TIER2
}
domain: {
  fqdn: "keep.google.com"
  tier: TIER2
}
domain: {
  fqdn: "manufacturers.google.com"
  tier: TIER2
}
domain: {
  fqdn: "maps.google.com"
  tier: TIER2
}
domain: {
  fqdn: "merchants.google.com"
  tier: TIER2
}
domain: {
  fqdn: "microhost.google.com"
  tier: TIER2
}
domain: {
  fqdn: "news.google.c
[0m
[0m$ [0mcurl -s "https://raw.githubusercontent.com/google/bughunters/main/cloud_tiers/cloud-tiers.asciipb" | head -c 10000
# proto-file: google3/cloud/security/pse/cloud_vrp/cptitool/cloud_tiers.proto
# proto-message: CloudTiers

# This file is effective from: 2026-05-01T00:00:00-08:00

product {
  name: "AlloyDB"
  tier: IT1
}
product {
  name: "App Engine Platform"
  tier: IT1
  remarks: "Web applications hosted on App Engine are out of scope."
  hostnames: "appengine.googleapis.com"
}
product {
  name: "Artifact Registry"
  tier: IT1
  hostnames: "artifactregistry.googleapis.com"
}
product {
  name: "BigQuery"
  tier: IT1
  hostnames: "bigquery.googleapis.com"
}
product {
  name: "Certificate Authority Service"
  tier: IT1
  hostnames: "privateca.googleapis.com"
}
product {
  name: "Cloud Bigtable"
  tier: IT1
  hostnames: "bigtableadmin.googleapis.com"
}
product {
  name: "Cloud Billing"
  tier: IT1
}
product {
  name: "Cloud Build"
  tier: IT1
  hostnames: "cloudbuild.googleapis.com"
}
product {
  name: "Cloud Identity and Access Management"
  tier: IT1
  hostnames: "iam.googleapis.com"
}
product {
  name: "Cloud Key Management Service"
  tier: IT1
  hostnames: "cloudkms.googleapis.com"
}
product {
  name: "Cloud Load Balancing"
  tier: IT1
}
product {
  name: "Cloud Pub/Sub"
  tier: IT1
  hostnames: "pubsub.googleapis.com"
}
product {
  name: "Cloud Run"
  tier: IT1
  hostnames: "run.googleapis.com"
}
product {
  name: "Cloud Spanner"
  tier: IT1
  hostnames: "spanner.googleapis.com"
  hostnames: "splodetestspanner.googleapis.com"
}
product {
  name: "Cloud SQL"
  tier: IT1
  hostnames: "sqladmin.googleapis.com"
}
product {
  name: "Cloud Storage"
  tier: IT1
  hostnames: "storage.googleapis.com"
}
product {
  name: "Compute Engine"
  tier: IT1
}
product {
  name: "Firestore"
  tier: IT1
  hostnames: "firestore.googleapis.com"
}
product {
  name: "GKE Identity Service"
  tier: IT1
}
product {
  name: "Google Cloud Console"
  tier: IT1
  hostnames: "console.cloud.google.com"
}
product {
  name: "Google Cloud Identity Platform"
  tier: IT1
  hostnames: "identitytoolkit.googleapis.com"
}
product {
  name: "Google Cloud Networking"
  tier: IT1
  hostnames: "servicenetworking.googleapis.com"
}
product {
  name: "Google Kubernetes Engine"
  tier: IT1
  hostnames: "container.googleapis.com"
}
product {
  name: "Memorystore"
  tier: IT1
}
product {
  name: "Secret Manager"
  tier: IT1
  hostnames: "secretmanager.googleapis.com"
}
product {
  name: "Vertex AI Platform"
  tier: IT1
  hostnames: "aiplatform.googleapis.com"
}
product {
  name: "Virtual Private Cloud"
  tier: IT1
}
product {
  name: "Agent Assist"
  tier: IT2
}
product {
  name: "Application Integration"
  tier: IT2
  hostnames: "integrations.googleapis.com"
}
product {
  name: "Artifact Analysis"
  tier: IT2
  hostnames: "containeranalysis.googleapis.com"
}
product {
  name: "Assured Workloads"
  tier: IT2
  hostnames: "assuredworkloads.googleapis.com"
}
product {
  name: "Binary Authorization"
  tier: IT2
  hostnames: "binaryauthorization.googleapis.com"
}
product {
  name: "Certificate Manager"
  tier: IT2
}
product {
  name: "Cloud API Gateway"
  tier: IT2
  hostnames: "apigateway.googleapis.com"
}
product {
  name: "Cloud CDN"
  tier: IT2
}
product {
  name: "Cloud Composer"
  tier: IT2
  hostnames: "composer.googleapis.com"
}
product {
  name: "Cloud Dataflow"
  tier: IT2
  hostnames: "dataflow.googleapis.com"
}
product {
  name: "Cloud Dataproc"
  tier: IT2
  hostnames: "dataproc.googleapis.com"
}
product {
  name: "Cloud Deploy"
  tier: IT2
  hostnames: "clouddeploy.googleapis.com"
}
product {
  name: "Cloud DNS"
  tier: IT2
}
product {
  name: "Cloud Interconnect"
  tier: IT2
}
product {
  name: "Cloud Logging"
  tier: IT2
  hostnames: "clouderrorreporting.googleapis.com"
  hostnames: "logging.googleapis.com"
}
product {
  name: "Cloud Monitoring"
  tier: IT2
  hostnames: "monitoring.googleapis.com"
}
product {
  name: "Cloud NGFW"
  tier: IT2
}
product {
  name: "Cloud Pub/Sub Lite"
  tier: IT2
  hostnames: "pubsublite.googleapis.com"
}
product {
  name: "Cloud Shell"
  tier: IT2
  hostnames: "cloudshell.googleapis.com"
}
product {
  name: "Cloud Source Repositories"
  tier: IT2
  hostnames: "sourcerepo.googleapis.com"
}
product {
  name: "Cloud Tasks"
  tier: IT2
  hostnames: "cloudtasks.googleapis.com"
}
product {
  name: "Cloud VPN"
  tier: IT2
}
product {
  name: "Conversational Insights"
  tier: IT2
  hostnames: "contactcenterinsights.googleapis.com"
}
product {
  name: "Dialogflow"
  tier: IT2
  hostnames: "dialogflow.googleapis.com"
}
product {
  name: "Document AI"
  tier: IT2
  hostnames: "documentai.googleapis.com"
}
product {
  name: "Easy APIs"
  tier: IT2
}
product {
  name: "Firebase Platform"
  tier: IT2
  hostnames: "fcm.googleapis.com"
  hostnames: "firebase.googleapis.com"
  hostnames: "firebasedatabase.googleapis.com"
}
product {
  name: "Gemini Cloud Assist"
  tier: IT2
}
product {
  name: "Gemini Code Assist"
  tier: IT2
}
product {
  name: "Genkit"
  tier: IT2
}
product {
  name: "GKE Enterprise on GCP"
  tier: IT2
}
product {
  name: "Google Cloud Armor"
  tier: IT2
}
product {
  name: "Google Cloud Marketplace"
  tier: IT2
}
product {
  name: "Google Distributed Cloud air-gapped"
  tier: IT2
}
product {
  name: "Google Distributed Cloud Edge"
  tier: IT2
  hostnames: "edgenetwork.googleapis.com"
}
product {
  name: "Google SecOps SIEM"
  tier: IT2
}
product {
  name: "Google SecOps SOAR"
  tier: IT2
}
product {
  name: "Healthcare Data Engine"
  tier: IT2
}
product {
  name: "Identity-Aware Proxy"
  tier: IT2
  hostnames: "iap.googleapis.com"
}
product {
  name: "Infrastructure Manager"
  tier: IT2
}
product {
  name: "Integration Connectors"
  tier: IT2
  hostnames: "connectors.googleapis.com"
}
product {
  name: "Investigator AI"
  tier: IT2
}
product {
  name: "Managed Service for Microsoft Active Directory"
  tier: IT2
  hostnames: "managedidentities.googleapis.com"
}
product {
  name: "NotebookLM for Enterprise"
  tier: IT2
}
product {
  name: "Private Service Connect"
  tier: IT2
}
product {
  name: "Privileged Access Manager"
  tier: IT2
}
product {
  name: "Recommendation Engine API"
  tier: IT2
}
product {
  name: "Resource Manager"
  tier: IT2
  hostnames: "cloudresourcemanager.googleapis.com"
}
product {
  name: "Secure Source Manager"
  tier: IT2
  hostnames: "securesourcemanager.googleapis.com"
}
product {
  name: "Security Command Center"
  tier: IT2
}
product {
  name: "Service Infrastructure"
  tier: IT2
}
product {
  name: "Service Usage"
  tier: IT2
  hostnames: "serviceusage.googleapis.com"
}
product {
  name: "Vertex AI Platform - IT2 subproducts"
  tier: IT2
  variants: "Vertex AI Feature Store"
  variants: "Vertex AI Model Monitoring"
  variants: "Vertex AI TensorBoard"
  variants: "Vertex AI Translation Hub"
  variants: "Vertex AI Vizier"
  variants: "Vertex ML Metadata"
  hostnames: "aiplatform.googleapis.com"
}
product {
  name: "Video Intelligence API"
  tier: IT2
  hostnames: "videointelligence.googleapis.com"
}
product {
  name: "Access Context Manager"
  tier: IT3
}
product {
  name: "Access Transparency"
  tier: IT3
}
product {
  name: "Active Assist"
  tier: IT3
}
product {
  name: "Anti Money Laundering AI"
  tier: IT3
}
product {
  name: "API Registry"
  tier: IT3
}
product {
  name: "Apigee API Platform"
  tier: IT3
  hostnames: "apigee.googleapis.com"
}
product {
  name: "App Hub"
  tier: IT3
}
product {
  name: "Application Design Center"
  tier: IT3
}
product {
  name: "AppSheet"
  tier: IT3
}
product {
  name: "Assured Open Source Software"
  tier: IT3
}
product {
  name: "Audit Manager"
  tier: IT3
}
product {
  name: "Backup and DR Service"
  tier: IT3
}
product {
  name: "Bare Metal HSM"
  tier: IT3
}
product {
  name: "Bare Metal Solution"
  tier: IT3
}
product {
  name: "Blockchain Analytics"
  tier: IT3
}
product {
  name: "Blockchain Node Engine"
  tier: IT3
  hostnames: "blockchainnodeengine.googleapis.com"
}
product {
  name: "Blockchain RPC"
  tier: IT3
}
product {
  name: "Blockchain Validator Manager"
  tier: IT3
}
product {
  name: "Chrome Enterprise Premium"
  tier: IT3
  hostnames: "beyondcorp.googleapis.com"
}
product {
  name: "Cloud Asset Inventory"
  tier: IT3
  hostnames: "cloudasset.googleapis.com"
}
product {
  name: "Cloud Data Fusion"
  tier: IT3
  hostnames: "datafusion.googleapis.com"
}
product {
  name: "Cloud Deployment Manager"
  tier: IT3
  hostnames: "deploymentmanager.googleapis.com"
}
product {
  name: "Cloud Domains"
  tier: IT3
}
product {
  name: "Cloud Endpoints"
  tier: IT3
}
product {
  name: "Cloud Healthcare API"
  tier: IT3
  hostnames: "healthcare.googleapis.com"
}
product {
  name: "Cloud Hub"
  tier: IT3
}
product {
  name: "Cloud Intrusion Detection System"
  tier: IT3
}
product {
  name: "Cloud Life Sciences"
  tier: IT3
}
product {
  name: "Cloud Location Finder"
  tier: IT3
}
product {
  name: "Cloud NAT"
  tier: IT3
}
product {
  name: "Cloud Natural Language API"
  tier: IT3
  hostnames: "language.googleapis.com"
}
product {
  name: "Cloud Profiler"
  tier: IT3
}
product {
  name: "Cloud Quotas"
  tier: IT3
}
product {
  name: "Cloud Router"
  tier: IT3
}
product {
  name: "Cloud Scheduler"
  tier: IT3
}
product {
  name: "Cloud SDK - Google Cloud CLI"
  tier: IT3
}
product {
  name: "Cloud Service Health"
  tier: IT3
}
product {
  name: "Cloud Service Health"
  tier: IT3
}
product {
  name: "Cloud Talent Solution"
  tier: IT3
  hostnames: "jobs.googleapis.com"
}
product {
  name: "Cloud Trace"
  tier: IT3
  hostnames: "cloudtrace.googleapis.com"
}
product {
  name: "Cloud Translation API"
  tier: IT3
  hostnames: "translate.googleapis.com"
}
product {
  name: "Cloud Vision API"
  tier: IT3
  hostnames: "vision.googleapis.com"
}
product {
  name: "Cloud Vision OCR On-Prem"
  tier: IT3
}
product {
  name: "Cloud Workstations"
  tier: IT3
  hostnames: "workstations.googleapis.com"
}
product {
  name: "Code Modernization for .NET"
  tier: IT3
}
product {
  name: "Colab Enterprise"
  tier: IT3
}
product {
  name: "Config Connector"
  tier: IT3
}
product
[0m
[0m$ [0mcurl -s "https://r.jina.ai/http://bughunters.google.com/about/rules/6521337925468160/google-open-source-software-vulnerability-reward-program-rules" | head -c 15000
Title: Google Open Source Software Vulnerability Reward Program Rules | Google Bug Hunters

URL Source: http://bughunters.google.com/about/rules/6521337925468160/google-open-source-software-vulnerability-reward-program-rules

Markdown Content:
Google’s Open Source Software Vulnerability Reward Program recognizes the contributions of security researchers who invest their time and effort in helping us secure open source software released by Google (Google OSS). Through this program, we provide monetary rewards and public recognition to researchers who disclose vulnerabilities in Google OSS to us.

## Repositories in scope [](https://bughunters.google.com/about/rules/open-source/google-open-source-software-vulnerability-reward-program-rules#repositories-in-scope)

The program covers all the latest versions of open source software stored in the public repositories of Google-owned GitHub organizations and selected repositories hosted on other platforms.

The program also covers repository configuration settings (e.g. GitHub actions, access control rules, GitHub application configurations).

### Third-party dependencies [](https://bughunters.google.com/about/rules/open-source/google-open-source-software-vulnerability-reward-program-rules#third-party-dependencies)

A critical element of the security of a software package is the security of its dependencies, so [vulnerabilities in 3rd-party dependencies](http://osv.dev/) are in scope for this program.

That said, please send your bug reports directly to the owner of the vulnerable package first and **ensure that the issue is addressed upstream before letting us know of the issue details**. Just like we would like to learn about vulnerabilities in our code first, we feel 3rd-party code authors should have the same advantage.

Submissions detailing vulnerabilities through 3rd-party dependencies should:

*   Demonstrate that the vulnerability manifests itself in our projects (i.e. you must show that the 3rd-party vulnerability can be triggered or exploited in Google OSS).
*   Be shared no earlier than 30 days after the issue was fixed upstream (e.g. a patched software package was released).

Vulnerabilities in 3rd-party _services_ or _platforms_ used to maintain and build Google OSS (e.g. source code management systems, CI/CD systems, package managers) are _out of scope for this program_. We cannot authorize you to conduct security research of assets that belong to other users and companies on their behalf. That said, we welcome submissions describing vulnerabilities in our configuration or integration of those 3rd-party services.

## Qualifying Vulnerabilities [](https://bughunters.google.com/about/rules/open-source/google-open-source-software-vulnerability-reward-program-rules#qualifying-vulnerabilities)

### Supply chain compromises [](https://bughunters.google.com/about/rules/open-source/google-open-source-software-vulnerability-reward-program-rules#supply-chain-compromises)

First and foremost, we welcome submissions pointing out vulnerabilities affecting source or build integrity that could result in a **supply chain compromise**. Supply chain vulnerabilities include the ability to compromise Google OSS source code, and build artifacts or packages distributed via package managers to users. For example:

*   Ability to modify or submit code on main branches of repositories
*   Vulnerabilities in the configuration of build and release infrastructure that lead to the compromise of artifacts that are distributed to users, e.g.:
    *   Vulnerabilities in Google OSS GitHub Actions configuration
    *   Insecure configuration of a project's GCP build environment setup
    *   Disclosure of package manager credentials for publishing build artifacts

*   Compromise of cryptographic signing keys for published artifacts

Important: You must be able to demonstrate that the vulnerability is exploitable, **bypassing the requirement that external contributors must first have PRs approved**. This can be achieved either by exploiting a vulnerability in an organization or repository that does not enforce this requirement, through a time-of-check-time-of-use (TOCTOU) vulnerability, or by other means. If your vulnerability will only trigger after approval by a maintainer, the vulnerability will be considered for "Credit" as it will be treated only as an insider risk.

### Product vulnerabilities [](https://bughunters.google.com/about/rules/open-source/google-open-source-software-vulnerability-reward-program-rules#product-vulnerabilities)

Any design or implementation issue in Google OSS that causes a **product [vulnerability](https://bughunters.google.com/learn/improving-your-reports/getting-started/6450430198153216/what-is-a-security-vulnerability)** substantially affecting the confidentiality or integrity of user data in software builds using Google OSS is also in scope for the program. Some examples:

Note: For vulnerabilities in Google's open source projects that are closely tied to Google Cloud or AI products, we encourage you to report them to the [Google Cloud Vulnerability Reward Program](https://bughunters.google.com/about/rules/google-friends/4849867320328192/cloud-vulnerability-reward-program-rules) or the [AI VRP](https://bughunters.google.com/about/rules/google-friends/5222232590712832/ai-vulnerability-reward-program-rules) to help us route your report to the right engineers.

*   Memory corruption issues in file format parsers or network protocol implementations
*   Failures in the sanitizer functions (e.g. HTML sanitizers)
*   Path traversal issues
*   Bad defaults or insecure code examples in project documentation

#### Acceptance criteria for product vulnerability type reports [](https://bughunters.google.com/about/rules/open-source/google-open-source-software-vulnerability-reward-program-rules#acceptance-criteria-for-product-vulnerability-type-reports)

The criteria for accepting product vulnerability type reports in the OSS VRP program depend on the project’s tier (OT0-OT3) and the subcategory of vulnerability, specifically:

*   All memory corruption vulnerabilities for **OT0** and **OT1** tier repositories require either exact [OSS-Fuzz reproduction steps](https://google.github.io/oss-fuzz/advanced-topics/reproducing/) (using an existing fuzz target, and including exact steps to [build the image](https://google.github.io/oss-fuzz/advanced-topics/reproducing/#building-using-docker)) or an already merged patch in the target repository.
*   Non-memory corruption vulnerabilities in **OT0** and **OT1** tier repositories do not require a merged patch for submission.
*   For all projects in the **OT2** (Standard) and **OT3** (Low-Priority) tiers, all types of "Product Vulnerabilities" are not eligible for monetary reward.

We aim to have our important projects (**OT0** and **OT1**) integrated with OSS-Fuzz when relevant, as this provides a more robust and scalable defense than triaging individual memory corruption issues; we prioritize rewards for researchers who contribute to this robust security model. For this reason, if a memory corruption vulnerability is found in an **OT0** or **OT1** project that lacks an existing or complete OSS-Fuzz integration, we encourage building the integration before sending the report to the VRP program.

Please note that **rewards are granted based on security impact alone**. For tiers where a merged patch is required for submission, the existence of a patch is a prerequisite but **not a guarantee of a reward**; the panel determines the overall security impact of the underlying issue at its own discretion.

### Other security issues [](https://bughunters.google.com/about/rules/open-source/google-open-source-software-vulnerability-reward-program-rules#other-security-issues)

We would also like to know about issues that affect the security of the target projects, but don't map to the above categories as they are not technical security vulnerabilities. For example:

*   Sensitive credentials that give write access which have been stored in personal projects (e.g. dotfiles)
*   Credential leaks in publicly stored backups
*   Weak passwords for 3rd-party CI systems for which we don't control the password policy
*   Insider risk, i.e. vulnerabilities that are only exploitable by maintainers or those with elevated permissions. There must be a realistic attack scenario for the attacks originating from non-maintainers. As a reminder, social engineering attacks are explicitly out of scope for rewards.

There are a couple of classes of vulnerabilities that generally do not qualify for a reward:

*   Security vulnerabilities and weaknesses with a root cause in downstream software integration with Google OSS (e.g. insecure configuration of Google OSS libraries, or third-party code calling Google OSS functions documented to accept sanitized inputs, with non-sanitized inputs).
*   Typosquatting / dependency confusion, unless it can be demonstrated how this leads to a compromise of Google OSS build artifacts distributed to users.
*   Insecure installation / software usage instructions that compromise the security of the developers working on the product
*   For all projects in the **OT2** (Standard) and **OT3** (Low-Priority) tiers, all types of "other security issues" are not eligible for monetary reward.
*   Issues with negligible security impact, as described in the [Bug Hunter University](https://bughunters.google.com/learn/invalid-reports/5374985771941888/about-this-section).

Out of concern for the availability of our services to all users, please do not attempt to carry out DoS attacks, leverage black hat SEO techniques, spam people, or perform other similarly questionable actions. Whenever possible, please try to test the vulnerabilities locally without impacting other users. We also discourage the use of any vulnerability testing tools that automatically generate significant volumes of traffic.

## Project tiers [](https://bughunters.google.com/about/rules/open-source/google-open-source-software-vulnerability-reward-program-rules#project-tiers)

Project tiers indicate the sensitivity and criticality connected with projects, and have implications for the reward amounts the VRP grants to researchers. The full list of **OT0** and **OT1** repositories can be found at **[https://github.com/google/bughunters](https://github.com/google/bughunters/blob/main/oss-repository-tier/external_repositories.txtpb)**.

### Flagship OSS projects (OT0) [](https://bughunters.google.com/about/rules/open-source/google-open-source-software-vulnerability-reward-program-rules#flagship-oss-projects-ot0-)

This tier contains selected Google open source projects that we consider particularly sensitive. Rewards for vulnerabilities found in projects in this tier are significantly higher than for projects in other tiers. The full and up-to-date list of **OT0** repositories can be found at **[https://github.com/google/bughunters](https://github.com/google/bughunters/blob/main/oss-repository-tier/external_repositories.txtpb)**.

### Important OSS projects (OT1) [](https://bughunters.google.com/about/rules/open-source/google-open-source-software-vulnerability-reward-program-rules#important-oss-projects-ot1-)

The Important (**OT1**) tier includes repositories with significant community impact or high security criticality. For a full and up-to-date list of **OT1** repositories, please refer to **[https://github.com/google/bughunters](https://github.com/google/bughunters/blob/main/oss-repository-tier/external_repositories.txtpb)**. Only the projects mentioned explicitly as **OT1** in this list fall under the **OT1** category.

### Standard OSS projects (OT2) [](https://bughunters.google.com/about/rules/open-source/google-open-source-software-vulnerability-reward-program-rules#standard-oss-projects-ot2-)

This tier contains all software repositories that are not explicitly mentioned in the criteria of other tiers. Typically, they fulfill all of the below requirements:

*   Repository is active and open to receive commits (i.e. not labeled as archived, deprecated, or in maintenance mode).
*   Repository stores code of a software library, framework, or end-user product.
*   Build artifacts from the repository are published in one of the popular package manager registries (e.g. npm, PyPI).
*   Released packages are marked as stable, release candidate, or late beta.

Note that there is no published list of **OT2** repositories and the tier decision when rewarding a report is at the discretion of the reward panel.

### Low-priority OSS projects (OT3) [](https://bughunters.google.com/about/rules/open-source/google-open-source-software-vulnerability-reward-program-rules#low-priority-oss-projects-ot3-)

This tier contains small, experimental, sample, and other low-priority projects. Projects in this tier typically have some combination of the following properties:

*   Non-existent or small community impact
*   No active development
*   Marked as "experimental", "demo", "sample", etc.
*   Unofficial project under a Google org
*   No executable code in the project (e.g. projects serving documentation websites)
*   Projects accompanying research projects conducted by Googlers
*   Repository belongs to "google-research", "googleinterns", "googlearchived", or similar GitHub orgs
*   No CI/CD setup on the repository

We currently do not financially reward submissions describing issues in projects from this tier.

## Reward amounts [](https://bughunters.google.com/about/rules/open-source/google-open-source-software-vulnerability-reward-program-rules#reward-amounts)

The following table outlines typical rewards for the most common classes of bugs, depending on the affected project tier.

| Category | OT0 (Flagship) | OT1 (Important) | OT2 (Standard) | OT3 (Low-priority) |
| --- | --- | --- | --- | --- |
| Supply chain compromises | $3,133.7 - $31,337 | $1,337 - $13,337 | $500 - $3,133.7 | - |
| Product vulnerabilities | $500 - $7,500 | $101 - $3,133.7 | - | - |
| Other security issues | $1,000 | $500 | - | - |

The final amount is always chosen at the discretion of the reward panel. In particular, we may decide to pay higher rewards for unusually clever, severe, or wide-reaching vulnerabilities; decide to pay lower rewards for vulnerabilities that hinge on the existence of other, not-yet-discovered or hypothetical bugs to become exploitable, require unusual user interaction or other rarely-met prerequisites; decide that a single report actually constitutes multiple bugs; or that multiple reports are so closely related that they only warrant a single reward.

For every reported vulnerability, the security impact is evaluated by looking at the most dangerous attack scenario that the panel can come up with. If we discover higher-impact attack vectors that the original reporter hadn't considered in the submitted report, we bump up the score accordingly. When receiving multiple reports, we typical
[0m
[0m$ [0mcurl -s "https://raw.githubusercontent.com/google/bughunters/main/oss-repository-tier/external_repositories.txtpb" | head -c 10000
repository {
  url: "https://github.com/angular/angular"
  tier: TIER_OT0
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/angular/dev-infra"
  tier: TIER_OT0
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/angular/angular-cli"
  tier: TIER_OT0
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/angular/components"
  tier: TIER_OT0
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/bazelbuild/bazel"
  tier: TIER_OT0
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/bazelbuild/bazel-central-registry"
  tier: TIER_OT0
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/bazelbuild/continuous-integration"
  tier: TIER_OT0
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/bazelbuild/remote-apis-sdks"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/google/gson"
  tier: TIER_OT0
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/protocolbuffers/protobuf"
  tier: TIER_OT0
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/openthread/openthread"
  tier: TIER_OT0
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/google/guava"
  tier: TIER_OT0
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/google/gvisor"
  tier: TIER_OT0
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/flutter/flutter"
  tier: TIER_OT0
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/flutter/devtools"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/GoogleContainerTools/distroless"
  tier: TIER_OT0
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/tink-crypto/tink-cc"
  tier: TIER_OT0
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/tink-crypto/tink-java"
  tier: TIER_OT0
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/tink-crypto/tink-go"
  tier: TIER_OT0
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/golang/go"
  tier: TIER_OT0
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/golang/net"
  tier: TIER_OT0
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/golang/vscode-go"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/golang/crypto"
  tier: TIER_OT0
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/golang/image"
  tier: TIER_OT0
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/golang/sync"
  tier: TIER_OT0
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/golang/text"
  tier: TIER_OT0
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/golang/tools"
  tier: TIER_OT0
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/golang/sys"
  tier: TIER_OT0
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/project-oak/oak"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/google/fonts"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/google/benchmark"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/google/clusterfuzz"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/google/re2"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/google/go-github"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/google/osv-scanner"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/cdapio/cdap"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_CLOUD_VRP
}
repository {
  url: "https://github.com/cdapio/cdap-ui"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_CLOUD_VRP
}
repository {
  url: "https://github.com/openxla/xla"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/google/go-cloud"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/google/closure-compiler"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/google/filament"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/google/flatbuffers"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/google/zerocopy"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/google/shaderc"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/jax-ml/jax"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/google/adk-python"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/google/adk-js"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/google/adk-go"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/google/adk-java"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/shaka-project/shaka-player"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/googlemaps/android-maps-utils"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/google/site-kit-wp"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/google/libphonenumber"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/google/brotli"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/Polymer/polymer"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/google/XNNPACK"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/google-gemini/gemini-cli"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_CLOUD_VRP
}
repository {
  url: "https://fuchsia.googlesource.com/fuchsia"
  tier: TIER_OT0
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/ChromeDevTools/chrome-devtools-mcp"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/tensorflow/tflite-micro"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/tensorflow/serving"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/tensorflow/models"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/tensorflow/tfjs"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/tensorflow/tensorflow"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/dart-lang/sdk"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/dart-lang/tools"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/dart-lang/test"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/dart-lang/build"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://gerrit.googlesource.com/gerrit"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/keras-team/keras"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://github.com/keras-team/tf-keras"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
repository {
  url: "https://gerrit.googlesource.com/gerrit"
  tier: TIER_OT1
  product_vuln_scope: SCOPE_OSS_VRP
}
[0m
[0m$ [0mcurl -s "https://api.github.com/repos/google/bughunters/contents" | jq -r '.[] | "\(.name) \(.type) \(.html_url)"'
CONTRIBUTING.md file https://github.com/google/bughunters/blob/main/CONTRIBUTING.md
LICENSE file https://github.com/google/bughunters/blob/main/LICENSE
README.md file https://github.com/google/bughunters/blob/main/README.md
cloud_tiers dir https://github.com/google/bughunters/tree/main/cloud_tiers
domain-tiers dir https://github.com/google/bughunters/tree/main/domain-tiers
oss-repository-tier dir https://github.com/google/bughunters/tree/main/oss-repository-tier
patch-rewards-program dir https://github.com/google/bughunters/tree/main/patch-rewards-program
[0m
[0m$ [0mcurl -s "https://api.github.com/repos/google/osv.dev/commits?per_page=5" | jq -r '.[] | "\(.sha) \(.commit.message)"'
...output truncated...

Full output saved to: /home/runner/.local/share/opencode/tool-output/tool_fdb7b3c6c0011nt7NQ4vLOFw0i


[Compare
Source](https://redirect.github.com/github/codeql-action/compare/v4.37.3...v4.37.4)

</details>

<details>
<summary>tcort/github-action-markdown-link-check
(tcort/github-action-markdown-link-check)</summary>

###
[`v1.1.3`](https://redirect.github.com/tcort/github-action-markdown-link-check/compare/v1.1.2...v1.1.3)

[Compare
Source](https://redirect.github.com/tcort/github-action-markdown-link-check/compare/v1.1.2...v1.1.3)

</details>

---

### Configuration

📅 **Schedule**: (in timezone Australia/Sydney)

- Branch creation
  - "before 6am on wednesday"
- Automerge
  - At any time (no schedule defined)

🚦 **Automerge**: Disabled by config. Please merge this manually once you
are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the
rebase/retry checkbox.

👻 **Immortal**: This PR will be recreated if closed unmerged. Get
[config
help](https://redirect.github.com/renovatebot/renovate/discussions) if
that's undesired.

---

- [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check
this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/).
View the [repository job
log](https://developer.mend.io/github/google/osv.dev).

<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0NC4xMS40IiwidXBkYXRlZEluVmVyIjoiNDQuMTIuMCIsInRhcmdldEJyYW5jaCI6Im1hc3RlciIsImxhYmVscyI6WyJkZXBlbmRlbmNpZXMiXX0=-->
269c96cf6cdaa14601c901df1f36fa5fef42ad0d chore(deps): update terraform to ~> 7.42.0 (#5777)

This PR contains the following updates:

| Package | Type | Update | Change | Pending |
|---|---|---|---|---|
| [google](https://registry.terraform.io/providers/hashicorp/google)
([source](https://redirect.github.com/hashicorp/terraform-provider-google))
| required_provider | minor | `~> 7.40.0` → `~> 7.42.0` | `7.43.0` |
|
[google-beta](https://registry.terraform.io/providers/hashicorp/google-beta)
([source](https://redirect.github.com/hashicorp/terraform-provider-google-beta))
| required_provider | minor | `~> 7.40.0` → `~> 7.42.0` | `7.43.0` |

---

### Release Notes

<details>
<summary>hashicorp/terraform-provider-google (google)</summary>

###
[`v7.42.0`](https://redirect.github.com/hashicorp/terraform-provider-google/releases/tag/v7.42.0)

[Compare
Source](https://redirect.github.com/hashicorp/terraform-provider-google/compare/v7.41.0...v7.42.0)

NOTES:

- compute: migrated `google_compute_region_instance_template` resource
to use direct HTTP rather than a client library
([#&#8203;28431](https://redirect.github.com/hashicorp/terraform-provider-google/pull/28431))

DEPRECATIONS:

- vertexai: deprecated `google_vertex_ai_schedule`, an
accidentally-added duplicate resource; use `google_colab_schedule`
instead.
([#&#8203;28406](https://redirect.github.com/hashicorp/terraform-provider-google/pull/28406))

FEATURES:

- **New Data Source:** `google_cloud_quotas_quota_adjuster_settings`
([#&#8203;28383](https://redirect.github.com/hashicorp/terraform-provider-google/pull/28383))
- **New List Resource:** `google_service_account_key`
([#&#8203;28430](https://redirect.github.com/hashicorp/terraform-provider-google/pull/28430))
- **New Resource:** `google_agent_identity_auth_provider`
([#&#8203;28447](https://redirect.github.com/hashicorp/terraform-provider-google/pull/28447))
- **New Resource:** `google_apihub_runtime_project_attachment`
([#&#8203;28449](https://redirect.github.com/hashicorp/terraform-provider-google/pull/28449))
- **New Resource:** `google_chronicle_big_query_export`
([#&#8203;28403](https://redirect.github.com/hashicorp/terraform-provider-google/pull/28403))
- **New Resource:** `google_compute_global_vm_extension_policy`
([#&#8203;28445](https://redirect.github.com/hashicorp/terraform-provider-google/pull/28445))
- **New Resource:** `google_compute_rollout_plan`
([#&#8203;28445](https://redirect.github.com/hashicorp/terraform-provider-google/pull/28445))
- **New Resource:** `google_vector_search_data_object`
([#&#8203;28434](https://redirect.github.com/hashicorp/terraform-provider-google/pull/28434))
- **New Resource:** `google_vertex_ai_persistent_resource`
([#&#8203;28435](https://redirect.github.com/hashicorp/terraform-provider-google/pull/28435))

IMPROVEMENTS:

- bigquery: added `table_type` field to `google_bigquery_routine`
resource
([#&#8203;28446](https://redirect.github.com/hashicorp/terraform-provider-google/pull/28446))
- cloudrunv2: added `start_execution_token` and `run_execution_token`
fields to `google_cloud_run_v2_job`resource
([#&#8203;28384](https://redirect.github.com/hashicorp/terraform-provider-google/pull/28384))
- colab: added `catch_up`, `create_pipeline_job_request`, `create_time`,
`last_pause_time`, `last_resume_time`, `last_scheduled_run_response`,
`max_concurrent_active_run_count`, `next_run_time`, `started_run_count`,
and `update_time` fields, and sub-fields under
`create_notebook_execution_job_request.notebook_execution_job`
(`create_time`, `custom_environment_spec`, `encryption_spec`,
`job_state`, `kernel_name`, `labels`, `name`, `schedule_resource_name`,
`workbench_runtime`) and under `create_notebook_execution_job_request`
(`notebook_execution_job_id`, `parent`) to `google_colab_schedule`
resource
([#&#8203;28406](https://redirect.github.com/hashicorp/terraform-provider-google/pull/28406))
- compute: added `effective_location` field to
`google_compute_interconnect` resource
([#&#8203;28416](https://redirect.github.com/hashicorp/terraform-provider-google/pull/28416))
- compute: added `request_headers` and `response_headers` fields to
`log_config` on `google_compute_backend_service` and
`google_compute_region_backend_service` resources
([#&#8203;28421](https://redirect.github.com/hashicorp/terraform-provider-google/pull/28421))
- compute: added identity support to `google_compute_instance`, allowing
resource import using an `identity` block
([#&#8203;28433](https://redirect.github.com/hashicorp/terraform-provider-google/pull/28433))
- compute: changed `location` field to mutable for
`google_compute_interconnect` resource
([#&#8203;28416](https://redirect.github.com/hashicorp/terraform-provider-google/pull/28416))
- container: added `addons_config.node_readiness_config` field to
`google_container_cluster` resource
([#&#8203;28417](https://redirect.github.com/hashicorp/terraform-provider-google/pull/28417))
- container: added `rollback_safe_upgrade`, `desired_emulated_version`,
and `emulated_version` fields to `google_container_cluster` resource
([#&#8203;28442](https://redirect.github.com/hashicorp/terraform-provider-google/pull/28442))
- container: increased default timeout to 2 hours for
`google_container_node_pool`resource
([#&#8203;28382](https://redirect.github.com/hashicorp/terraform-provider-google/pull/28382))
- dataproc: added `confidential_instance_type` field to
`google_dataproc_cluster` resource
([#&#8203;28371](https://redirect.github.com/hashicorp/terraform-provider-google/pull/28371))
- gkehub: added `min_control_plane_version`, `min_node_version`,
`target_control_plane_version`, `target_node_version`, and
`operational_state` fields to `google_gke_hub_rollout_sequence` resource
([#&#8203;28429](https://redirect.github.com/hashicorp/terraform-provider-google/pull/28429))
- hypercomputecluster: increased default timeouts for
`google_hypercomputecluster_cluster` to 120 minutes
([#&#8203;28448](https://redirect.github.com/hashicorp/terraform-provider-google/pull/28448))
- modelarmor: added field `template_metadata.filter_version_selector` to
`google_model_armor_template` resource
([#&#8203;28402](https://redirect.github.com/hashicorp/terraform-provider-google/pull/28402))
- sql: added identity support to `google_sql_user` for `terraform query`
support
([#&#8203;28428](https://redirect.github.com/hashicorp/terraform-provider-google/pull/28428))

BUG FIXES:

- bigtable: fixed an issue where `bigtable_custom_endpoint` and
`universe_domain` were ignored when creating Bigtable resources
([#&#8203;28404](https://redirect.github.com/hashicorp/terraform-provider-google/pull/28404))
- compute: fixed an issue where diffs in
`google_compute_security_policy` were not detected
([#&#8203;28420](https://redirect.github.com/hashicorp/terraform-provider-google/pull/28420))
- gkehub: fixed `rollout_creation_scope` and `upgrade_types` fields in
`google_gke_hub_rollout_sequence` resource
([#&#8203;28429](https://redirect.github.com/hashicorp/terraform-provider-google/pull/28429))
- osconfig: added client-side validation to ensure
`resource_hierarchy_selector` and `location_selector` are not set at the
same time in `google_os_config_v2_policy_orchestrator`,
`google_os_config_v2_policy_orchestrator_for_folder`, and
`google_os_config_v2_policy_orchestrator_for_organization`
([#&#8203;28407](https://redirect.github.com/hashicorp/terraform-provider-google/pull/28407))
- secretmanager: fixed an issue where
`google_secret_manager_secret_version` would fail at apply time if
neither `secret_data` nor `secret_data_wo` was set
([#&#8203;28419](https://redirect.github.com/hashicorp/terraform-provider-google/pull/28419))
- sql: fixed issue where updates to
`settings.ip_configuration.psc_config.allowed_consumer_projects` in
`google_sql_database_instance` were silently ignored on in-place updates
([#&#8203;28444](https://redirect.github.com/hashicorp/terraform-provider-google/pull/28444))
- vertexai: fixed
`google_vertex_ai_endpoint_with_model_garden_deployment` destroying and
recreating the endpoint when `min_replica_count`, `max_replica_count`,
`required_replica_count`, or `autoscaling_metric_specs` changed
([#&#8203;28401](https://redirect.github.com/hashicorp/terraform-provider-google/pull/28401))

###
[`v7.41.0`](https://redirect.github.com/hashicorp/terraform-provider-google/blob/HEAD/CHANGELOG.md#7410-Unreleased)

[Compare
Source](https://redirect.github.com/hashicorp/terraform-provider-google/compare/v7.40.0...v7.41.0)

</details>

<details>
<summary>hashicorp/terraform-provider-google-beta
(google-beta)</summary>

###
[`v7.42.0`](https://redirect.github.com/hashicorp/terraform-provider-google-beta/releases/tag/v7.42.0)

[Compare
Source](https://redirect.github.com/hashicorp/terraform-provider-google-beta/compare/v7.41.0...v7.42.0)

NOTES:

- compute: migrated `google_compute_region_instance_template` resource
to use direct HTTP rather than a client library
([#&#8203;12752](https://redirect.github.com/hashicorp/terraform-provider-google-beta/pull/12752))

DEPRECATIONS:

- vertexai: deprecated `google_vertex_ai_schedule`, an
accidentally-added duplicate resource; use `google_colab_schedule`
instead.
([#&#8203;12739](https://redirect.github.com/hashicorp/terraform-provider-google-beta/pull/12739))

FEATURES:

- **New List Resource:** `google_service_account_key`
([#&#8203;12750](https://redirect.github.com/hashicorp/terraform-provider-google-beta/pull/12750))
- **New Resource:** `google_agent_identity_auth_provider`
([#&#8203;12762](https://redirect.github.com/hashicorp/terraform-provider-google-beta/pull/12762))
- **New Resource:** `google_apihub_runtime_project_attachment`
([#&#8203;12764](https://redirect.github.com/hashicorp/terraform-provider-google-beta/pull/12764))
- **New Resource:** `google_chronicle_big_query_export`
([#&#8203;12737](https://redirect.github.com/hashicorp/terraform-provider-google-beta/pull/12737))
- **New Resource:** `google_vector_search_data_object`
([#&#8203;12755](https://redirect.github.com/hashicorp/terraform-provider-google-beta/pull/12755))
- **New Resource:** `google_vertex_ai_persistent_resource`
([#&#8203;12756](https://redirect.github.com/hashicorp/terraform-provider-google-beta/pull/12756))

IMPROVEMENTS:

- bigquery: added `table_type` field to `google_bigquery_routine`
resource
([#&#8203;12761](https://redirect.github.com/hashicorp/terraform-provider-google-beta/pull/12761))
- colab: added `catch_up`, `create_pipeline_job_request`, `create_time`,
`last_pause_time`, `last_resume_time`, `last_scheduled_run_response`,
`max_concurrent_active_run_count`, `next_run_time`, `started_run_count`,
`update_time` fields, and sub-fields under
`create_notebook_execution_job_request.notebook_execution_job`
(`create_time`, `custom_environment_spec`, `encryption_spec`,
`job_state`, `kernel_name`, `labels`, `name`, `schedule_resource_name`,
`workbench_runtime`) and under `create_notebook_execution_job_request`
(`notebook_execution_job_id`, `parent`) to `google_colab_schedule`
resource
([#&#8203;12739](https://redirect.github.com/hashicorp/terraform-provider-google-beta/pull/12739))
- composer: added `config.node_config.traffic_routing_config` field to
`google_composer_environment` resource
([#&#8203;12733](https://redirect.github.com/hashicorp/terraform-provider-google-beta/pull/12733))
- compute: added `effective_location` field to
`google_compute_interconnect` resource
([#&#8203;12742](https://redirect.github.com/hashicorp/terraform-provider-google-beta/pull/12742))
- compute: added `request_headers` and `response_headers` fields to
`log_config` on `google_compute_backend_service` and
`google_compute_region_backend_service` resources
([#&#8203;12746](https://redirect.github.com/hashicorp/terraform-provider-google-beta/pull/12746))
- compute: added identity support to `google_compute_instance`, allowing
resource import using an `identity` block
([#&#8203;12754](https://redirect.github.com/hashicorp/terraform-provider-google-beta/pull/12754))
- compute: changed `location` field to mutable for
`google_compute_interconnect` resource
([#&#8203;12742](https://redirect.github.com/hashicorp/terraform-provider-google-beta/pull/12742))
- container: added `addons_config.node_readiness_config` field to
`google_container_cluster` resource
([#&#8203;12743](https://redirect.github.com/hashicorp/terraform-provider-google-beta/pull/12743))
- container: added `rollback_safe_upgrade`, `desired_emulated_version`,
and `emulated_version` fields to `google_container_cluster` resource
([#&#8203;12757](https://redirect.github.com/hashicorp/terraform-provider-google-beta/pull/12757))
- container: increased default timeout to 2 hours for
`google_container_node_pool` resource
([#&#8203;12727](https://redirect.github.com/hashicorp/terraform-provider-google-beta/pull/12727))
- dataproc: added `confidential_instance_type` field to
`google_dataproc_cluster` resource
([#&#8203;12725](https://redirect.github.com/hashicorp/terraform-provider-google-beta/pull/12725))
- gkehub: added `min_control_plane_version`, `min_node_version`,
`target_control_plane_version`, `target_node_version`, and
`operational_state` fields to `google_gke_hub_rollout_sequence` resource
([#&#8203;12749](https://redirect.github.com/hashicorp/terraform-provider-google-beta/pull/12749))
- hypercomputecluster: increased default timeouts for
`google_hypercomputecluster_cluster` to 120 minutes
([#&#8203;12763](https://redirect.github.com/hashicorp/terraform-provider-google-beta/pull/12763))
- modelarmor: added `template_metadata.filter_version_selector` field to
`google_model_armor_template` resource
([#&#8203;12736](https://redirect.github.com/hashicorp/terraform-provider-google-beta/pull/12736))
- sql: added identity support to `google_sql_user` for `terraform query`
support
([#&#8203;12748](https://redirect.github.com/hashicorp/terraform-provider-google-beta/pull/12748))

BUG FIXES:

- bigtable: fixed an issue where `bigtable_custom_endpoint` and
`universe_domain` were ignored when creating Bigtable resources
([#&#8203;12738](https://redirect.github.com/hashicorp/terraform-provider-google-beta/pull/12738))
- compute: fixed an issue where diffs in
`google_compute_security_policy` were not detected
([#&#8203;12745](https://redirect.github.com/hashicorp/terraform-provider-google-beta/pull/12745))
- gkehub: fixed `rollout_creation_scope` and `upgrade_types` fields in
`google_gke_hub_rollout_sequence` resource
([#&#8203;12749](https://redirect.github.com/hashicorp/terraform-provider-google-beta/pull/12749))
- osconfig: added client-side validation to ensure
`resource_hierarchy_selector` and `location_selector` are not set at the
same time in `google_os_config_v2_policy_orchestrator`,
`google_os_config_v2_policy_orchestrator_for_folder`, and
`google_os_config_v2_policy_orchestrator_for_organization`
([#&#8203;12740](https://redirect.github.com/hashicorp/terraform-provider-google-beta/pull/12740))
- secretmanager: fixed an issue where
`google_secret_manager_secret_version` would fail at apply time if
neither `secret_data` nor `secret_data_wo` was set
([#&#8203;12744](https://redirect.github.com/hashicorp/terraform-provider-google-beta/pull/12744))
- sql: fixed issue where updates to
`settings.ip_configuration.psc_config.allowed_consumer_projects` in
`google_sql_database_instance` were silently ignored on in-place updates
([#&#8203;12759](https://redirect.github.com/hashicorp/terraform-provider-google-beta/pull/12759))
- vertexai: fixed
`google_vertex_ai_endpoint_with_model_garden_deployment` destroying and
recreating the endpoint when `min_replica_count`, `max_replica_count`,
`required_replica_count`, or `autoscaling_metric_specs` changed
([#&#8203;12735](https://redirect.github.com/hashicorp/terraform-provider-google-beta/pull/12735))

###
[`v7.41.0`](https://redirect.github.com/hashicorp/terraform-provider-google-beta/blob/HEAD/CHANGELOG.md#7410-Unreleased)

[Compare
Source](https://redirect.github.com/hashicorp/terraform-provider-google-beta/compare/v7.40.0...v7.41.0)

</details>

---

### Configuration

📅 **Schedule**: (in timezone Australia/Sydney)

- Branch creation
  - "before 6am on wednesday"
- Automerge
  - At any time (no schedule defined)

🚦 **Automerge**: Disabled by config. Please merge this manually once you
are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the
rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about these
updates again.

---

- [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check
this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/).
View the [repository job
log](https://developer.mend.io/github/google/osv.dev).

<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0NC4xMS40IiwidXBkYXRlZEluVmVyIjoiNDQuMTIuMCIsInRhcmdldEJyYW5jaCI6Im1hc3RlciIsImxhYmVscyI6WyJkZXBlbmRlbmNpZXMiXX0=-->
46fa6ea8522f7a92ac6bd10d5d29d3244ba655a2 chore(deps): update pnpm to v11.18.0 (#5776)

This PR contains the following updates:

| Package | Change |
[Age](https://docs.renovatebot.com/merge-confidence/) |
[Confidence](https://docs.renovatebot.com/merge-confidence/) |
|---|---|---|---|
| [pnpm](https://pnpm.io)
([source](https://redirect.github.com/pnpm/pnpm/tree/HEAD/pnpm11/pnpm))
| [`11.15.1` →
`11.18.0`](https://renovatebot.com/diffs/npm/pnpm/11.15.1/11.18.0) |
![age](https://developer.mend.io/api/mc/badges/age/npm/pnpm/11.18.0?slim=true)
|
![confidence](https://developer.mend.io/api/mc/badges/confidence/npm/pnpm/11.15.1/11.18.0?slim=true)
|

---

### Release Notes

<details>
<summary>pnpm/pnpm (pnpm)</summary>

###
[`v11.18.0`](https://redirect.github.com/pnpm/pnpm/releases/tag/v11.18.0):
pnpm 11.18

[Compare
Source](https://redirect.github.com/pnpm/pnpm/compare/v11.17.0...v11.18.0)

#### Minor Changes

- Fixed an installed optional dependency being left without one of its
own required dependencies. When a package reached through
`optionalDependencies` is installable on the current system but one of
its regular `dependencies` is not, a lockfile-based install skipped that
dependency and installed the parent anyway, so importing the parent
failed with `MODULE_NOT_FOUND`. The dependency is now installed, and an
install-check warning reports the incompatibility. A dependency is still
only skipped when every path to it is optional, or when the package that
pulls it in was itself skipped
[#&#8203;13286](https://redirect.github.com/pnpm/pnpm/issues/13286).

- `pnpm setup` now appends `PNPM_HOME` and the global bin directory to
the GitHub Actions environment files (`GITHUB_ENV` and `GITHUB_PATH`),
so later steps in the same job can run `pnpm add --global` and other
global commands
[#&#8203;9191](https://redirect.github.com/pnpm/pnpm/issues/9191).

- Added support for `publishConfig.name`, which publishes a package
under a different name than the one its manifest carries in the
workspace. It is for a project whose published name is already taken by
a sibling project, which otherwise has to be renamed by a build step
just before publishing. Only the published artifact is renamed —
dependents, `pnpm-lock.yaml`, and release tooling keep addressing the
project by its manifest name — and the new name reaches the packed
manifest, the tarball filename, and everything that addresses the
package at the registry: the already-published check of `pnpm publish
-r`, its registry selection, and the release-planning probes of `pnpm
change status` and `pnpm version -r`
[#&#8203;13345](https://redirect.github.com/pnpm/pnpm/issues/13345).

- `pnpm self-update` no longer takes any instruction from the project it
is run in:

- pnpm is fetched through the same trusted registry and auth
configuration used when switching pnpm versions, so a project `.npmrc`
or `pnpm-workspace.yaml` can no longer redirect the download or attach
credentials to it, and the project's default `.pnpmfile.(c|m)js` is no
longer loaded. Pnpmfiles from trusted sources (the `pnpmfile` setting,
the global pnpmfile, config dependencies) still apply.
- The `minimumReleaseAge` settings in `pnpm-workspace.yaml` no longer
affect `self-update`. They still govern the project's own dependencies;
for `self-update` the cooldown now comes from the built-in default, your
global config, a `PNPM_CONFIG_*` environment variable, or a command-line
flag. This fixes `self-update` failing inside a workspace that raises
the cutoff while succeeding everywhere else, and stops a repository from
either waiving the cooldown or keeping you on an outdated pnpm by
raising it.
- The same applies to the `trustPolicy` settings and to `ci`: a project
can no longer weaken the trust check that guards the pnpm download, nor
re-enable the confirmation prompt that a CI run suppresses.

When `self-update` refuses a version that is younger than the cutoff, an
interactive run now offers to update anyway; non-interactive runs still
fail. CI never prompts, even on a runner that attaches a TTY.

#### Patch Changes

- Fixed `pnpm licenses list` to report every version when the same
package is installed under multiple aliases
[pnpm/pnpm#13438](https://redirect.github.com/pnpm/pnpm/issues/13438).

- Sort `pnpm dedupe --check` snapshot changes for stable output across
pnpm implementations.

- Strip Unicode formatting characters from registry- and
manifest-derived terminal output.

- Speed up installs after compatible catalog or direct dependency range
changes by retaining the locked version without resolving the dependency
graph again.

- Speed up installs after safe override changes by reusing unambiguous
compatible dependency resolutions, pruning obsolete dependencies,
applying independent replacements and removals together, and handling
parent-scoped `"-"` overrides without full lockfile resolution.

- Installing a local `file:` directory dependency with the global
virtual store enabled no longer fails with `TypeError: Cannot read
properties of undefined (reading 'split')`
[#&#8203;13335](https://redirect.github.com/pnpm/pnpm/issues/13335).

Local directory dependencies — `file:` directories and injected
workspace packages — now get a global-virtual-store slot of their own
per project. They used to share one slot across every project that
depended on a directory of the same name, so a project could end up
linked to another project's copy of the dependency.

- The `Workspace` column of `pnpm update --interactive` now falls back
to the project's path when its `name` is only whitespace, as it already
did for a missing or empty one — all three render an equally blank label
otherwise.

- Checking GitHub Actions dependencies for updates is now opt-in for
every command. Neither `pnpm outdated` nor `pnpm update` reads the
workflow files unless `--include-github-actions` is passed or
`update.githubActions` is set to `true` in `pnpm-workspace.yaml`.
Reading them runs `git ls-remote` against every referenced repository,
which fails in environments where GitHub is not reachable the way pnpm
assumes (a GitHub Enterprise Server, a custom certificate authority, or
an offline network)
[#&#8203;13254](https://redirect.github.com/pnpm/pnpm/issues/13254).

  `pnpm outdated` accepts the `--include-github-actions` option too.

- `pnpm update --interactive` now measures its table in terminal columns
rather than in characters. A package name, workspace name, or version
containing wide characters (CJK, most emoji) no longer knocks its row's
columns out of line with the rest of the group, and a wide character in
a version no longer aborts the command with `Subject parameter value
width cannot be greater than the container width`
[#&#8203;13357](https://redirect.github.com/pnpm/pnpm/issues/13357).

- The `Workspace` column of `pnpm update --interactive` is more
informative in two cases. A dependency outdated at the same version in
several workspace projects is offered as one choice, since selecting it
updates every project — that choice now names all of them instead of
only the first. And a workspace project without a `name` is now labelled
with its path rather than left blank, so several unnamed projects can be
told apart.

- An auto-installed *optional* peer is no longer hoisted at a version
the workspace root's own dependency on that package excludes.
`resolvePeersFromWorkspaceRoot` already made the workspace root's
specifier decide which version a missing *required* peer is installed
at; the optional-peer picker ignored it and always took the highest
version present anywhere in the graph. In a workspace whose root pins
`postcss: 8.5.10`, an importer that depends on `webpack` and declares no
`postcss` of its own got `postcss@8.5.22` hoisted for
`terser-webpack-plugin`'s optional `postcss` peer, leaving two
`postcss@8.5.x` instances in the graph
[#&#8203;13320](https://redirect.github.com/pnpm/pnpm/issues/13320).

- `overrides` now also govern peers that pnpm auto-installs. Previously
an override only rewrote dependencies declared in a manifest, so a peer
nobody declares — installed because `autoInstallPeers` is on — resolved
against its declared peer range and could bring in a second copy of the
very package the override pinned. For example, with `overrides: { react:
npm:react@19.2.0 }` and a lone `lucide-react` dependency, pnpm installed
`react@18.3.1`; it now installs the pinned `react@19.2.0`
[#&#8203;13320](https://redirect.github.com/pnpm/pnpm/issues/13320).

- Under `resolvePeersFromWorkspaceRoot`, a workspace root dependency
declared with `link:` or `file:` (or the path form of `workspace:`, such
as `workspace:../pkg`) now satisfies another project's missing peer
dependency at the linked package's own version, instead of being hoisted
as a path. Those specifiers are relative to the project that declares
them, so the same specifier reached a different directory — or none —
from the project the peer was hoisted into, leaving a broken link. The
root now has the same authority over the peer as it has when it declares
the package with a version range
[#&#8203;13373](https://redirect.github.com/pnpm/pnpm/issues/13373).

- Installs through a pnpr server now apply the project's whole
verification policy. `minimumReleaseAgeExclude`,
`minimumReleaseAgeIgnoreMissingTime`, `trustPolicy`,
`trustPolicyExclude`, `trustPolicyIgnoreAfter`, and `trustLockfile` were
ignored, so excluded packages were still held back and a lockfile
containing them could be rejected.

`trustPolicy: no-downgrade` no longer fails with
`TRUST_POLICY_INCOMPATIBLE_WITH_PNPR` when a pnpr server is configured.

`--frozen-lockfile` and `--no-prefer-frozen-lockfile` are now honored on
the pnpr path, instead of resolving and rewriting the lockfile anyway.
Since `frozenLockfile` defaults to `true` on CI, a CI install through a
pnpr server now fails on an out-of-date lockfile rather than updating
it.

- Workspace installs through a pnpr server no longer crash with `Cannot
read properties of undefined (reading 'filter')` after linking, when
`minimumReleaseAge` is active
[#&#8203;13275](https://redirect.github.com/pnpm/pnpm/issues/13275).

- Fixed `pnpm dedupe` updating valid catalog resolutions when another
matching version exists in the lockfile.

- `pnpm -r run "/pattern/" --no-bail` no longer exits zero when one of a
project's matched scripts fails and a later one passes. The run summary
carries a single status per project, and the passing script overwrote
the recorded failure.

- Restored the store block a first install prints, naming how packages
were materialized and where the stores live
[#&#8203;13315](https://redirect.github.com/pnpm/pnpm/issues/13315):

  ```text
Packages are hard linked from the content-addressable store to the
virtual store.
    Content-addressable store is at: ~/.local/share/pnpm/store/v11
    Virtual store is at:             node_modules/.pnpm
  ```

- The root project's `pnpm:devPreinstall` script now runs before
resolution and linking, as it does in pnpm 11. It is skipped under
`--ignore-scripts`, `--lockfile-only` and `--dry-run`, by `pnpm fetch`
and `pnpm rebuild`, and by a repeat install that is already up to date.
Workspaces that use the hook to prepare state the install depends on —
such as [next.js](https://redirect.github.com/vercel/next.js), which
generates a placeholder `next` bin with it — were left with dependents
linked against files that were never created
[#&#8203;13313](https://redirect.github.com/pnpm/pnpm/issues/13313).

- Prevented `pnpm dedupe --check` from removing an incompatible
`node_modules` directory.

- `pnpm update --workspace` no longer links dependencies the user never
named:

- Running it with `updateConfig.ignoreDependencies` configured no longer
fails with `ERR_PNPM_WORKSPACE_PACKAGE_NOT_FOUND` for a dependency that
is only published to the registry. Such dependencies keep their
specifiers, as they already did when no dependencies were ignored.
- Passing package selectors that match no direct dependency no longer
falls back to linking every workspace dependency.

<!-- sponsors -->

#### Platinum Sponsors

<table>
  <tbody>
    <tr>
      <td align="center" valign="middle">
<a href="https://bit.cloud/?utm_source=pnpm&utm_medium=release_notes"
target="_blank" rel="noopener noreferrer"><img
src="https://pnpm.io/img/users/bit.svg" width="80" alt="Bit"></a>
      </td>
    </tr>
    <tr>
      <td align="center" valign="middle">
<a href="https://openai.com/?utm_source=pnpm&utm_medium=release_notes"
target="_blank" rel="noopener noreferrer">
          <picture>
<source media="(prefers-color-scheme: light)"
srcset="https://pnpm.io/img/users/openai_dark.svg" />
<source media="(prefers-color-scheme: dark)"
srcset="https://pnpm.io/img/users/openai_light.svg" />
<img src="https://pnpm.io/img/users/openai_dark.svg" width="160"
alt="OpenAI" />
          </picture>
        </a>
      </td>
    </tr>
  </tbody>
</table>

#### Gold Sponsors

<table>
  <tbody>
    <tr>
      <td align="center" valign="middle">
<a href="https://sanity.io/?utm_source=pnpm&utm_medium=release_notes"
target="_blank" rel="noopener noreferrer">
          <picture>
<source media="(prefers-color-scheme: light)"
srcset="https://pnpm.io/img/users/sanity.svg" />
<source media="(prefers-color-scheme: dark)"
srcset="https://pnpm.io/img/users/sanity_light.svg" />
<img src="https://pnpm.io/img/users/sanity.svg" width="120" alt="Sanity"
/>
          </picture>
        </a>
      </td>
      <td align="center" valign="middle">
<a href="https://discord.com/?utm_source=pnpm&utm_medium=release_notes"
target="_blank" rel="noopener noreferrer">
          <picture>
<source media="(prefers-color-scheme: light)"
srcset="https://pnpm.io/img/users/discord.svg" />
<source media="(prefers-color-scheme: dark)"
srcset="https://pnpm.io/img/users/discord_light.svg" />
<img src="https://pnpm.io/img/users/discord.svg" width="220"
alt="Discord" />
          </picture>
        </a>
      </td>
      <td align="center" valign="middle">
<a href="https://vite.dev/?utm_source=pnpm&utm_medium=release_notes"
target="_blank" rel="noopener noreferrer"><img
src="https://pnpm.io/img/users/vitejs.svg" width="42" alt="Vite"></a>
      </td>
    </tr>
    <tr>
      <td align="center" valign="middle">
<a href="https://serpapi.com/?utm_source=pnpm&utm_medium=release_notes"
target="_blank" rel="noopener noreferrer">
          <picture>
<source media="(prefers-color-scheme: light)"
srcset="https://pnpm.io/img/users/serpapi_dark.svg" />
<source media="(prefers-color-scheme: dark)"
srcset="https://pnpm.io/img/users/serpapi_light.svg" />
<img src="https://pnpm.io/img/users/serpapi_dark.svg" width="160"
alt="SerpApi" />
          </picture>
        </a>
      </td>
      <td align="center" valign="middle">
<a
href="https://coderabbit.ai/?utm_source=pnpm&utm_medium=release_notes"
target="_blank" rel="noopener noreferrer">
          <picture>
<source media="(prefers-color-scheme: light)"
srcset="https://pnpm.io/img/users/coderabbit.svg" />
<source media="(prefers-color-scheme: dark)"
srcset="https://pnpm.io/img/users/coderabbit_light.svg" />
<img src="https://pnpm.io/img/users/coderabbit.svg" width="220"
alt="CodeRabbit" />
          </picture>
        </a>
      </td>
      <td align="center" valign="middle">
<a
href="https://stackblitz.com/?utm_source=pnpm&utm_medium=release_notes"
target="_blank" rel="noopener noreferrer">
          <picture>
<source media="(prefers-color-scheme: light)"
srcset="https://pnpm.io/img/users/stackblitz.svg" />
<source media="(prefers-color-scheme: dark)"
srcset="https://pnpm.io/img/users/stackblitz_light.svg" />
<img src="https://pnpm.io/img/users/stackblitz.svg" width="190"
alt="Stackblitz" />
          </picture>
        </a>
      </td>
    </tr>
    <tr>
      <td align="center" valign="middle">
<a href="https://workleap.com/?utm_source=pnpm&utm_medium=release_notes"
target="_blank" rel="noopener noreferrer">
          <picture>
<source media="(prefers-color-scheme: light)"
srcset="https://pnpm.io/img/users/workleap.svg" />
<source media="(prefers-color-scheme: dark)"
srcset="https://pnpm.io/img/users/workleap_light.svg" />
<img src="https://pnpm.io/img/users/workleap.svg" width="190"
alt="Workleap" />
          </picture>
        </a>
      </td>
      <td align="center" valign="middle">
<a href="https://nx.dev/?utm_source=pnpm&utm_medium=release_notes"
target="_blank" rel="noopener noreferrer">
          <picture>
<source media="(prefers-color-scheme: light)"
srcset="https://pnpm.io/img/users/nx.svg" />
<source media="(prefers-color-scheme: dark)"
srcset="https://pnpm.io/img/users/nx_light.svg" />
<img src="https://pnpm.io/img/users/nx.svg" width="50" alt="Nx" />
          </picture>
        </a>
      </td>
    </tr>
  </tbody>
</table>

<!-- sponsors end -->

###
[`v11.17.0`](https://redirect.github.com/pnpm/pnpm/releases/tag/v11.17.0):
pnpm 11.17

[Compare
Source](https://redirect.github.com/pnpm/pnpm/compare/v11.16.0...v11.17.0)

##### Minor Changes

- Added a new setting, `update.githubActionsServer`, for specifying the
base URL of the GitHub server that hosts the repositories of the GitHub
Actions referenced by the workflow files (for example, a GitHub
Enterprise Server). When the setting is not defined, the URL is read
from the `GITHUB_SERVER_URL` environment variable, falling back to
`https://github.com`. The URL must use the `https://` or `http://`
protocol
[#&#8203;13220](https://redirect.github.com/pnpm/pnpm/issues/13220).

`pnpm outdated` and `pnpm update` no longer fail when the refs of a
GitHub Action's repository cannot be read (for example, when the
action's repository is private or hosted on a different GitHub server).
Such actions are now skipped with a warning.

Setting `update.githubActions` to `false` now makes `pnpm outdated` and
the interactive `pnpm update` skip GitHub Actions dependencies.

##### Patch Changes

- The token poll for web-based authentication no longer reads the body
of non-OK or still-pending (HTTP 202) responses, and caps the token
response body it does read at 64 KiB, so a malicious or compromised
registry cannot exhaust memory through the poll
[pnpm/pnpm#12721](https://redirect.github.com/pnpm/pnpm/issues/12721).

- Fixed `catalog:` references in dependencies and overrides failing to
resolve when installing through a pnpr server, which errored with "No
catalog entry '<name>' was found for catalog 'default'." even though the
catalog entry existed. Also fixed a crash on Windows when installing a
nested workspace member (e.g. `packages/foo`) through a pnpr server
[#&#8203;13232](https://redirect.github.com/pnpm/pnpm/issues/13232).

- Republished every package: the tarballs published by the v11.13.1
through v11.16.0 releases were missing most of their compiled files due
to a packing bug
[#&#8203;13164](https://redirect.github.com/pnpm/pnpm/issues/13164).

- Revert script ordering change for `pnpm run --sequential /regex/`

- Support the `from-git` argument in the `pnpm version` command.

- When the authentication URL cannot be rendered as a QR code (for
example when it exceeds the maximum QR data capacity), web-based login
now displays the URL alone with a warning instead of aborting
authentication
[pnpm/pnpm#12721](https://redirect.github.com/pnpm/pnpm/issues/12721).

<!-- sponsors -->

##### Platinum Sponsors

<table>
  <tbody>
    <tr>
      <td align="center" valign="middle">
<a href="https://bit.cloud/?utm_source=pnpm&utm_medium=release_notes"
target="_blank" rel="noopener noreferrer"><img
src="https://pnpm.io/img/users/bit.svg" width="80" alt="Bit"></a>
      </td>
    </tr>
    <tr>
      <td align="center" valign="middle">
<a href="https://openai.com/?utm_source=pnpm&utm_medium=release_notes"
target="_blank" rel="noopener noreferrer">
          <picture>
<source media="(prefers-color-scheme: light)"
srcset="https://pnpm.io/img/users/openai_dark.svg" />
<source media="(prefers-color-scheme: dark)"
srcset="https://pnpm.io/img/users/openai_light.svg" />
<img src="https://pnpm.io/img/users/openai_dark.svg" width="160"
alt="OpenAI" />
          </picture>
        </a>
      </td>
    </tr>
  </tbody>
</table>

##### Gold Sponsors

<table>
  <tbody>
    <tr>
      <td align="center" valign="middle">
<a href="https://sanity.io/?utm_source=pnpm&utm_medium=release_notes"
target="_blank" rel="noopener noreferrer">
          <picture>
<source media="(prefers-color-scheme: light)"
srcset="https://pnpm.io/img/users/sanity.svg" />
<source media="(prefers-color-scheme: dark)"
srcset="https://pnpm.io/img/users/sanity_light.svg" />
<img src="https://pnpm.io/img/users/sanity.svg" width="120" alt="Sanity"
/>
          </picture>
        </a>
      </td>
      <td align="center" valign="middle">
<a href="https://discord.com/?utm_source=pnpm&utm_medium=release_notes"
target="_blank" rel="noopener noreferrer">
          <picture>
<source media="(prefers-color-scheme: light)"
srcset="https://pnpm.io/img/users/discord.svg" />
<source media="(prefers-color-scheme: dark)"
srcset="https://pnpm.io/img/users/discord_light.svg" />
<img src="https://pnpm.io/img/users/discord.svg" width="220"
alt="Discord" />
          </picture>
        </a>
      </td>
      <td align="center" valign="middle">
<a href="https://vite.dev/?utm_source=pnpm&utm_medium=release_notes"
target="_blank" rel="noopener noreferrer"><img
src="https://pnpm.io/img/users/vitejs.svg" width="42" alt="Vite"></a>
      </td>
    </tr>
    <tr>
      <td align="center" valign="middle">
<a href="https://serpapi.com/?utm_source=pnpm&utm_medium=release_notes"
target="_blank" rel="noopener noreferrer">
          <picture>
<source media="(prefers-color-scheme: light)"
srcset="https://pnpm.io/img/users/serpapi_dark.svg" />
<source media="(prefers-color-scheme: dark)"
srcset="https://pnpm.io/img/users/serpapi_light.svg" />
<img src="https://pnpm.io/img/users/serpapi_dark.svg" width="160"
alt="SerpApi" />
          </picture>
        </a>
      </td>
      <td align="center" valign="middle">
<a
href="https://coderabbit.ai/?utm_source=pnpm&utm_medium=release_notes"
target="_blank" rel="noopener noreferrer">
          <picture>
<source media="(prefers-color-scheme: light)"
srcset="https://pnpm.io/img/users/coderabbit.svg" />
<source media="(prefers-color-scheme: dark)"
srcset="https://pnpm.io/img/users/coderabbit_light.svg" />
<img src="https://pnpm.io/img/users/coderabbit.svg" width="220"
alt="CodeRabbit" />
          </picture>
        </a>
      </td>
      <td align="center" valign="middle">
<a
href="https://stackblitz.com/?utm_source=pnpm&utm_medium=release_notes"
target="_blank" rel="noopener noreferrer">
          <picture>
<source media="(prefers-color-scheme: light)"
srcset="https://pnpm.io/img/users/stackblitz.svg" />
<source media="(prefers-color-scheme: dark)"
srcset="https://pnpm.io/img/users/stackblitz_light.svg" />
<img src="https://pnpm.io/img/users/stackblitz.svg" width="190"
alt="Stackblitz" />
          </picture>
        </a>
      </td>
    </tr>
    <tr>
      <td align="center" valign="middle">
<a href="https://workleap.com/?utm_source=pnpm&utm_medium=release_notes"
target="_blank" rel="noopener noreferrer">
          <picture>
<source media="(prefers-color-scheme: light)"
srcset="https://pnpm.io/img/users/workleap.svg" />
<source media="(prefers-color-scheme: dark)"
srcset="https://pnpm.io/img/users/workleap_light.svg" />
<img src="https://pnpm.io/img/users/workleap.svg" width="190"
alt="Workleap" />
          </picture>
        </a>
      </td>
      <td align="center" valign="middle">
<a href="https://nx.dev/?utm_source=pnpm&utm_medium=release_notes"
target="_blank" rel="noopener noreferrer">
          <picture>
<source media="(prefers-color-scheme: light)"
srcset="https://pnpm.io/img/users/nx.svg" />
<source media="(prefers-color-scheme: dark)"
srcset="https://pnpm.io/img/users/nx_light.svg" />
<img src="https://pnpm.io/img/users/nx.svg" width="50" alt="Nx" />
          </picture>
        </a>
      </td>
    </tr>
  </tbody>
</table>

<!-- sponsors end -->

###
[`v11.16.0`](https://redirect.github.com/pnpm/pnpm/releases/tag/v11.16.0):
pnpm 11.16

[Compare
Source](https://redirect.github.com/pnpm/pnpm/compare/v11.15.1...v11.16.0)

#### Minor Changes

- The first release of a package now publishes the version written in
its manifest verbatim, instead of bumping off it. `pnpm version -r` and
`pnpm change status` check the registry for each release's current
version; when that version is not yet published, the package debuts at
it and its pending changesets apply only from the next release. A newly
added package seeded at `1100.0.0` with a `minor` changeset is therefore
published as `1100.0.0` rather than skipping straight to `1100.1.0`.

- Added a `--changeset` flag to `pnpm update`. Set `update.changeset` to
`true` in `pnpm-workspace.yaml` to enable this behavior by default, and
use `--no-changeset` to override the setting for one update. After the
update completes, pnpm writes a `.changeset/pnpm-update-<suffix>.md`
file declaring a patch bump for every workspace package whose
`dependencies` or `optionalDependencies` were changed by the update and
a major bump when `peerDependencies` changed, including packages that
consume an updated catalog entry via the `catalog:` protocol. Private
packages, packages without a name, and packages listed in the `ignore`
array of `.changeset/config.json` are skipped. If
`.changeset/config.json` does not exist, a warning is printed and no
changeset is generated.

- Added GitHub Actions dependencies to `pnpm outdated` and interactive
`pnpm update`. Non-interactive updates can include them with
`--include-github-actions` or by setting `update.githubActions` to
`true` in `pnpm-workspace.yaml`. Updated actions are pinned to exact
commit hashes with their release tags preserved in comments.

- Added `update` and `audit` settings sections to `pnpm-workspace.yaml`,
superseding the awkwardly named `updateConfig`, `auditConfig`, and
top-level `auditLevel` settings:

  ```yaml
  update:
    ignoreDeps: # was updateConfig.ignoreDependencies
      - webpack
      - "@babel/*"

  audit:
    level: high # was auditLevel
    ignore: # was auditConfig.ignoreGhsas
      - GHSA-xxxx-yyyy-zzzz
  ```

`update.ignoreDeps` lists dependency name patterns that `pnpm update`
and `pnpm outdated` should skip. `audit.level` and `audit.ignore` tune
`pnpm audit`.

The deprecated `updateConfig`, `auditConfig`, and `auditLevel` settings
keep working until the next major version. When both a new section value
and its deprecated counterpart are set, the new section takes precedence
and a warning is printed. Both the TypeScript CLI and the Rust config
surface (pacquet) recognize the new sections.

#### Patch Changes

- Fixed `pnpm add --save-exact`/`--save-prefix` and `pnpm update`
writing a package's version with the `peerDependencies` range's prefix
(e.g. `^19.2.7` instead of the requested `19.2.7`) whenever the same
package also appeared in `peerDependencies`. A real
`dependencies`/`devDependencies`/`optionalDependencies` entry now takes
precedence over a same-named `peerDependencies` entry when computing the
current specifiers
[#&#8203;13108](https://redirect.github.com/pnpm/pnpm/issues/13108).

<!-- sponsors -->

#### Platinum Sponsors

<table>
  <tbody>
    <tr>
      <td align="center" valign="middle">
<a href="https://bit.cloud/?utm_source=pnpm&utm_medium=release_notes"
target="_blank" rel="noopener noreferrer"><img
src="https://pnpm.io/img/users/bit.svg" width="80" alt="Bit"></a>
      </td>
    </tr>
    <tr>
      <td align="center" valign="middle">
<a href="https://openai.com/?utm_source=pnpm&utm_medium=release_notes"
target="_blank" rel="noopener noreferrer">
          <picture>
<source media="(prefers-color-scheme: light)"
srcset="https://pnpm.io/img/users/openai_dark.svg" />
<source media="(prefers-color-scheme: dark)"
srcset="https://pnpm.io/img/users/openai_light.svg" />
<img src="https://pnpm.io/img/users/openai_dark.svg" width="160"
alt="OpenAI" />
          </picture>
        </a>
      </td>
    </tr>
  </tbody>
</table>

#### Gold Sponsors

<table>
  <tbody>
    <tr>
      <td align="center" valign="middle">
<a href="https://sanity.io/?utm_source=pnpm&utm_medium=release_notes"
target="_blank" rel="noopener noreferrer">
          <picture>
<source media="(prefers-color-scheme: light)"
srcset="https://pnpm.io/img/users/sanity.svg" />
<source media="(prefers-color-scheme: dark)"
srcset="https://pnpm.io/img/users/sanity_light.svg" />
<img src="https://pnpm.io/img/users/sanity.svg" width="120" alt="Sanity"
/>
          </picture>
        </a>
      </td>
      <td align="center" valign="middle">
<a href="https://discord.com/?utm_source=pnpm&utm_medium=release_notes"
target="_blank" rel="noopener noreferrer">
          <picture>
<source media="(prefers-color-scheme: light)"
srcset="https://pnpm.io/img/users/discord.svg" />
<source media="(prefers-color-scheme: dark)"
srcset="https://pnpm.io/img/users/discord_light.svg" />
<img src="https://pnpm.io/img/users/discord.svg" width="220"
alt="Discord" />
          </picture>
        </a>
      </td>
      <td align="center" valign="middle">
<a href="https://vite.dev/?utm_source=pnpm&utm_medium=release_notes"
target="_blank" rel="noopener noreferrer"><img
src="https://pnpm.io/img/users/vitejs.svg" width="42" alt="Vite"></a>
      </td>
    </tr>
    <tr>
      <td align="center" valign="middle">
<a href="https://serpapi.com/?utm_source=pnpm&utm_medium=release_notes"
target="_blank" rel="noopener noreferrer">
          <picture>
<source media="(prefers-color-scheme: light)"
srcset="https://pnpm.io/img/users/serpapi_dark.svg" />
<source media="(prefers-color-scheme: dark)"
srcset="https://pnpm.io/img/users/serpapi_light.svg" />
<img src="https://pnpm.io/img/users/serpapi_dark.svg" width="160"
alt="SerpApi" />
          </picture>
        </a>
      </td>
      <td align="center" valign="middle">
<a
href="https://coderabbit.ai/?utm_source=pnpm&utm_medium=release_notes"
target="_blank" rel="noopener noreferrer">
          <picture>
<source media="(prefers-color-scheme: light)"
srcset="https://pnpm.io/img/users/coderabbit.svg" />
<source media="(prefers-color-scheme: dark)"
srcset="https://pnpm.io/img/users/coderabbit_light.svg" />
<img src="https://pnpm.io/img/users/coderabbit.svg" width="220"
alt="CodeRabbit" />
          </picture>
        </a>
      </td>
      <td align="center" valign="middle">
<a
href="https://stackblitz.com/?utm_source=pnpm&utm_medium=release_notes"
target="_blank" rel="noopener noreferrer">
          <picture>
<source media="(prefers-color-scheme: light)"
srcset="https://pnpm.io/img/users/stackblitz.svg" />
<source media="(prefers-color-scheme: dark)"
srcset="https://pnpm.io/img/users/stackblitz_light.svg" />
<img src="https://pnpm.io/img/users/stackblitz.svg" width="190"
alt="Stackblitz" />
          </picture>
        </a>
      </td>
    </tr>
    <tr>
      <td align="center" valign="middle">
<a href="https://workleap.com/?utm_source=pnpm&utm_medium=release_notes"
target="_blank" rel="noopener noreferrer">
          <picture>
<source media="(prefers-color-scheme: light)"
srcset="https://pnpm.io/img/users/workleap.svg" />
<source media="(prefers-color-scheme: dark)"
srcset="https://pnpm.io/img/users/workleap_light.svg" />
<img src="https://pnpm.io/img/users/workleap.svg" width="190"
alt="Workleap" />
          </picture>
        </a>
      </td>
      <td align="center" valign="middle">
<a href="https://nx.dev/?utm_source=pnpm&utm_medium=release_notes"
target="_blank" rel="noopener noreferrer">
          <picture>
<source media="(prefers-color-scheme: light)"
srcset="https://pnpm.io/img/users/nx.svg" />
<source media="(prefers-color-scheme: dark)"
srcset="https://pnpm.io/img/users/nx_light.svg" />
<img src="https://pnpm.io/img/users/nx.svg" width="50" alt="Nx" />
          </picture>
        </a>
      </td>
    </tr>
  </tbody>
</table>

<!-- sponsors end -->

</details>

---

### Configuration

📅 **Schedule**: (in timezone Australia/Sydney)

- Branch creation
  - "before 6am on wednesday"
- Automerge
  - At any time (no schedule defined)

🚦 **Automerge**: Disabled by config. Please merge this manually once you
are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the
rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update
again.

---

- [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check
this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/).
View the [repository job
log](https://developer.mend.io/github/google/osv.dev).

<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0NC4xMS40IiwidXBkYXRlZEluVmVyIjoiNDQuMTIuMCIsInRhcmdldEJyYW5jaCI6Im1hc3RlciIsImxhYmVscyI6WyJkZXBlbmRlbmNpZXMiXX0=-->
[0m
[0m$ [0mcurl -s "https://api.github.com/repos/google/osv-scanner/commits?per_page=5" | jq -r '.[] | "\(.sha) \(.commit.message)"'
add393a2e051ea46b7d94aa96e468bbdde1e621d test: add E2E test for openSUSE Leap ecosystem mapping (#2937)

## Summary

Adds an end-to-end test for the openSUSE Leap ecosystem mapping fix that
just merged in
[google/osv-scalibr#2290](https://github.com/google/osv-scalibr/pull/2290)
(closes
[google/osv-scalibr#2203](https://github.com/google/osv-scalibr/issues/2203)),
following the same pattern used for the AlmaLinux (#2870) and Mageia
(#2873) ecosystem E2E tests.

- Adds a pinned `opensuse/leap:15.5` image fixture
(`test-opensuse-leap-15.5.Dockerfile`).
- Adds `Scanning_openSUSE_Leap_15.5_image` /
`scanning_opensuse_leap_15.5_image` cases to `TestCommand_OCIImage` and
`TestCommand_OCIImage_JSONFormat`, with recorded cassette entries and
snapshots showing real SUSE-SU advisories detected for `curl` and
`openssl-1_1`.

While wiring this up I found the `os/rpm` extractor was never registered
in the `artifact` preset used by image scanning
(`internal/scalibrplugin/presets.go`), so any RPM-based image, not just
openSUSE Leap, currently scans with zero package inventory ("No package
sources found"). This is the same root cause already fixed for AlmaLinux
in #2870 (commit f995ea59), which hasn't landed on `main` yet, so I
included the same registration here to unblock this test. Updated the
snapshots/cassettes it touches (`scan/source`, `internal/output`,
`internal/scalibrplugin`).

`go.mod` points at a branch on my osv-scalibr fork carrying just the
openSUSE Leap ecosystem fix cherry-picked onto the same osv-scalibr base
already used by the AlmaLinux/Mageia branches. Bumping straight to the
latest osv-scalibr `main` pulls in unrelated breaking API changes
(`PluginConfig` refactor, Maven client signature changes, etc.) that
osv-scanner hasn't absorbed yet. This is the same temporary-replace
pattern used while #2250 was in flight; happy to drop it once either
this or the AlmaLinux/Mageia scalibr bump lands.

## Test plan

- [x] `go build ./...`: clean
- [x] `go test ./... -short`: all packages pass
- [x] `TEST_ACCEPTANCE=true go test ./cmd/osv-scanner/scan/image/...
-run 'TestCommand_OCIImage$|TestCommand_OCIImage_JSONFormat$'`: all
pass, including the new openSUSE Leap cases
- [x] Verified the JSON snapshot shows real detections: `curl
8.0.1-150400.5.50.1` matches `SUSE-SU-2024:3926-1` and
`SUSE-SU-2024:4359-1`; `openssl-1_1 1.1.1l-150500.17.34.1` matches
`SUSE-SU-2024:3765-1`; ecosystem `openSUSE:Leap 15.5`; base image
`opensuse/leap` correctly detected.


cc @Ly-Joey: this follows up on your review request on
google/osv-scalibr#2290 to add E2E coverage for the openSUSE Leap
ecosystem mapping.

---------

Co-authored-by: Rex P <rexpan@google.com>
Co-authored-by: Rex P <106129829+another-rex@users.noreply.github.com>
656f850a76a4ea41439d91dfbc16bf3b846eb490 chore(deps): update actions/stale action to v11 (#2961)

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [actions/stale](https://redirect.github.com/actions/stale) | action |
major | `v10.4.0` → `v11.0.0` |

---

### Release Notes

<details>
<summary>actions/stale (actions/stale)</summary>

###
[`v11.0.0`](https://redirect.github.com/actions/stale/releases/tag/v11.0.0)

[Compare
Source](https://redirect.github.com/actions/stale/compare/v11.0.0...v11.0.0)

#### What's Changed

##### Enhancement

- Migrate to ESM and update dependencies by
[@&#8203;chiranjib-swain](https://github-grid.enterprise.slack.com/team/U08CVLQ4JKE)
in [#&#8203;1350](https://redirect.github.com/actions/stale/pull/1350)

##### Dependency Update

- Override brace-expansion to 5.0.8 to address 24 high-severity
dependency vulnerabilities by
[@&#8203;dependabot](https://redirect.github.com/dependabot) in
[#&#8203;1351](https://redirect.github.com/actions/stale/pull/1351)

**Full Changelog**:
<https://github.com/actions/stale/compare/v10...v11.0.0>

###
[`v11`](https://redirect.github.com/actions/stale/compare/v10.4.0...v11.0.0)

[Compare
Source](https://redirect.github.com/actions/stale/compare/v10.4.0...v11.0.0)

</details>

---

### Configuration

📅 **Schedule**: (in timezone Australia/Sydney)

- Branch creation
  - "before 6am on monday"
- Automerge
  - At any time (no schedule defined)

🚦 **Automerge**: Disabled by config. Please merge this manually once you
are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the
rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update
again.

---

- [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check
this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/).
View the [repository job
log](https://developer.mend.io/github/google/osv-scanner).

<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0NC4xMS40IiwidXBkYXRlZEluVmVyIjoiNDQuMTIuMCIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiZGVwZW5kZW5jaWVzIl19-->
a258868211a57052da6bd323f758b8388dee02bb fix: Attempt 3/4 fix macos test timeouts (#2958)

Fix macos test timeouts
0a11aa246772af0f21e3ce3ca46492a207b2763e chore(deps): update workflows (#2959)

This PR contains the following updates:

| Package | Type | Update | Change | Pending |
|---|---|---|---|---|
| [docker/login-action](https://redirect.github.com/docker/login-action)
| action | minor | `v4.5.1` → `v4.6.0` | |
|
[github/codeql-action](https://redirect.github.com/github/codeql-action)
| action | patch | `v4.37.3` → `v4.37.4` | `v4.37.6` (+1) |
|
[tcort/github-action-markdown-link-check](https://redirect.github.com/tcort/github-action-markdown-link-check)
| action | patch | `v1.1.2` → `v1.1.3` | |

---

### Release Notes

<details>
<summary>docker/login-action (docker/login-action)</summary>

###
[`v4.6.0`](https://redirect.github.com/docker/login-action/compare/v4.5.2...v4.6.0)

[Compare
Source](https://redirect.github.com/docker/login-action/compare/v4.5.2...v4.6.0)

###
[`v4.5.2`](https://redirect.github.com/docker/login-action/compare/v4.5.1...v4.5.2)

[Compare
Source](https://redirect.github.com/docker/login-action/compare/v4.5.1...v4.5.2)

</details>

<details>
<summary>github/codeql-action (github/codeql-action)</summary>

###
[`v4.37.4`](https://redirect.github.com/github/codeql-action/compare/v4.37.3...v4.37.4)

[Compare
Source](https://redirect.github.com/github/codeql-action/compare/v4.37.3...v4.37.4)

</details>

<details>
<summary>tcort/github-action-markdown-link-check
(tcort/github-action-markdown-link-check)</summary>

###
[`v1.1.3`](https://redirect.github.com/tcort/github-action-markdown-link-check/compare/v1.1.2...v1.1.3)

[Compare
Source](https://redirect.github.com/tcort/github-action-markdown-link-check/compare/v1.1.2...v1.1.3)

</details>

---

### Configuration

📅 **Schedule**: (in timezone Australia/Sydney)

- Branch creation
  - "before 6am on monday"
- Automerge
  - At any time (no schedule defined)

🚦 **Automerge**: Disabled by config. Please merge this manually once you
are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the
rebase/retry checkbox.

👻 **Immortal**: This PR will be recreated if closed unmerged. Get
[config
help](https://redirect.github.com/renovatebot/renovate/discussions) if
that's undesired.

---

- [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check
this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/).
View the [repository job
log](https://developer.mend.io/github/google/osv-scanner).

<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0NC4xMS40IiwidXBkYXRlZEluVmVyIjoiNDQuMTIuMCIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiZGVwZW5kZW5jaWVzIl19-->
c32b46d20c305c1f3826792737ad1c932708edfb chore(deps): lock file maintenance (#2962)

This PR contains the following updates:

| Update | Change |
|---|---|
| lockFileMaintenance | All locks refreshed |

🔧 This Pull Request updates lock files to use the latest dependency
versions.

---

### Configuration

📅 **Schedule**: (in timezone Australia/Sydney)

- Branch creation
  - "before 4am on monday"
- Automerge
  - At any time (no schedule defined)

🚦 **Automerge**: Disabled by config. Please merge this manually once you
are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the
rebase/retry checkbox.

👻 **Immortal**: This PR will be recreated if closed unmerged. Get
[config
help](https://redirect.github.com/renovatebot/renovate/discussions) if
that's undesired.

---

- [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check
this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/).
View the [repository job
log](https://developer.mend.io/github/google/osv-scanner).

<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0NC4xMS40IiwidXBkYXRlZEluVmVyIjoiNDQuMTEuNCIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiZGVwZW5kZW5jaWVzIl19-->

Co-authored-by: Rex P <106129829+another-rex@users.noreply.github.com>
[0m
[0m$ [0mcurl -s "https://api.github.com/repos/google/tsunami-security-scanner/commits?per_page=5" | jq -r '.[] | "\(.sha) \(.commit.message)"'
363ba87b3543f8ae8e4304d3416818f03da7f262 Merge pull request #156 from doyensec:new-templated-variables

PiperOrigin-RevId: 936702330
Change-Id: Ie86d25b85468311d0ba8885ca3e2e76e218ffd21
238a4277e15c1484a7c02adfab89102febbb7d59 fixed test case
e4e8d6ec1a71f2f7c31d5522c11493c0f1380f20 No public description

PiperOrigin-RevId: 930528155
Change-Id: Id4313d24e9efe264f89ca73d39cebf2fbdf23a54
c723152d4de8291e49fc98de492dacf33b3ab1fa Fix asyncio loop creation for Python 3.14

PiperOrigin-RevId: 927208016
Change-Id: Id67afc0e7eb9e43539960769e63d5e2d75cd084e
b0b443f9d4b9c6ea33637a1627f386d6aa67d00f changed test cases
[0m
# Program: google
# Phase: RECON
## Key Findings (one line each, specific)
- Google VRP covers 6 distinct programs: Google/Alphabet VRP (web), Cloud VRP, AI VRP, Android/Devices VRP, Chrome/ChromeOS VRP, OSS VRP — each with separate rules, scopes, and reward tables
- Scope includes *.google.com, *.youtube.com, *.blogger.com, *.deepmind.com, *.waymo.com, *.wing.com + Google/Waymo iOS apps; Cloud VRP covers all GCP products (IT1/IT2/IT3 tiers); AI VRP covers Flagship (Search, Gemini, Workspace core), Standard (AI Studio, Jules), and Other tiers
- Domain tiers published: 50+ TIER0 domains (accounts.google.com, console.cloud.google.com, gemini.google.com, mail.google.com, etc.), 100+ TIER1, 200+ TIER2 — TIER0/1 are highest reward multipliers
- Certificate transparency reveals 200+ historical subdomains including corp.google.com, sandbox.google.com, *.c.docs.google.com, *.clients*.google.com, adwords regional variants, and legacy/internal hosts (orkut, wifi, freezone, flexpack)
- Google GitHub org has 100+ public repos; security-relevant: security-research (pocs for 11 vendor categories), osv.dev (vuln DB), osv-scanner (Go vuln scanner), tsunami-security-scanner (Java/Go scanner), atheris (Python fuzzer), bughunters (tier configs)
- OSS VRP tiers published: OT0 (Flagship: angular, bazel, protobuf, golang/go, tensorflow, flutter, guava, gvisor, tink-crypto) and OT1 (Important: adk-*, filament, flatbuffers, jax, XNNPACK, shaka-player, etc.) — supply chain compromise rewards up to $31,337

## New Endpoints / Assets
- TIER0 domains: accounts.google.com, admin.google.com, bard.google.com, chrome.google.com, console.cloud.google.com, gemini.google.com, mail.google.com, myaccount.google.com, passwords.google.com, play.google.com, takeout.google.com, vault.google.com, console.firebase.google.com, issuetracker.google.com, cloudsearch.google.com
- Cloud IT1 products: AlloyDB, App Engine, Artifact Registry, BigQuery, Cloud Build, Cloud KMS, Cloud Run, Cloud SQL, Cloud Storage, Compute Engine, Firestore, GKE, Secret Manager, Vertex AI, Cloud Console, IAM, VPC, Load Balancing, Pub/Sub, Spanner, Bigtable
- Historical subdomains from CT: *.corp.google.com, *.sandbox.google.com, *.c.docs.google.com, *.clients*.google.com, *.ext.google.com, *.vp.video.l.google.com, adwords.google.com.*, onex.wifi.google.com, hosted-id.google.com, freezone.google.com, flexpack.google.com

## Hypotheses (numbered, with impact + read-only test)
1. **OAuth redirect_uri validation on TIER0 auth endpoints** — accounts.google.com, myaccount.google.com, admin.google.com handle sensitive auth flows; misconfigured redirect_uri allowlist could enable account takeover via OAuth code theft. Test: GET /.well-known/oauth-authorization-server on TIER0 domains, compare allowlist against known Google OAuth clients.
2. **Cloud Console (console.cloud.google.com) IAM policy evaluation bypass** — TIER0 domain with IT1 Cloud IAM product; potential for privilege escalation via malformed condition expressions or resource hierarchy traversal. Test: GET /api/cloudresourcemanager/v1/projects/{id}:getIamPolicy on test project (requires auth — read-only design only).
3. **AI Studio (aistudio.google.com) / Gemini (gemini.google.com) indirect prompt injection via shared context** — AI VRP Flagship tier, S1/S2 categories ($20k/$15k); context manipulation across user sessions via shared documents/drive. Test: GET /api/contentgeneration/v1/models/gemini-pro:generateContent with crafted prompt referencing victim's Drive (requires auth — design only).
4. **OSS supply chain compromise via GitHub Actions in OT0 repos** — angular/angular, bazelbuild/bazel, golang/go use complex CI; compromised action or malicious PR could inject backdoor into released artifacts. Test: Review .github/workflows/*.yml in OT0 repos for unpinned actions, self-hosted runner risks, secret exposure in logs.
5. **Subdomain takeover on deprecated *.corp.google.com / *.sandbox.google.com hosts** — CT shows many legacy subdomains; if DNS records point to deprovisioned GCP resources (App Engine, Cloud Run), takeover possible. Test: DNS resolve for each CT subdomain, check CNAME targets against known GCP service patterns (appspot.com, run.app, cloudfunctions.net).
6. **Vertex AI (aiplatform.googleapis.com) model deployment SSRF via custom container** — IT1 product, allows custom containers; malicious image could access metadata server or internal services. Test: Review Vertex AI custom container docs for network isolation guarantees (public source review only).
7. **Secret leakage in public repos via Renovate/Dependabot PRs** — osv.dev, osv-scanner show automated dependency PRs with lockfile updates; potential for malicious package substitution if supply chain compromised. Test: Audit recent Renovate PRs in google/osv.dev, google/osv-scanner for unexpected version pins or new dependencies.

## Secrets Check (public repos: what was found, hash only, where)
- No hardcoded secrets found in recent commits of google/osv.dev, google/osv-scanner, google/tsunami-security-scanner, google/security-research — commits show routine dependency updates, test additions, and CI maintenance
- Renovate bot PRs in osv.dev/osv-scanner only update Terraform providers, pnpm, GitHub Actions — no API keys, tokens, or credentials in diffs
- google/security-research/pocs/ contains only vulnerability PoCs for third-party software (LibRaw, Apple, Azure, etc.) — no Google internal secrets

## CVSS Candidates
- OAuth redirect_uri bypass on TIER0: CVSS 9.1 (AV:N/AC:L/PR:N/UI:R/S:C/C:H/I:H/A:N) — account takeover via code theft
- Cloud Console IAM privilege escalation: CVSS 8.8 (AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H) — project/org takeover
- AI indirect prompt injection (S1 Rogue Actions): CVSS 8.2 (AV:N/AC:L/PR:N/UI:R/S:U/C:H/I:H/A:L) — sensitive data exfiltration / state change
- OSS supply chain compromise (OT0): CVSS 9.8 (AV:N/AC:L/PR:N/UI:N/S:C/C:H/I:H/A:H) — malicious artifact distribution
- Subdomain takeover on legacy *.corp.google.com: CVSS 7.5 (AV:N/AC:L/PR:N/UI:N/S:U/C:N/I:H/A:N) — phishing / brand abuse
- Vertex AI custom container SSRF: CVSS 8.6 (AV:N/AC:L/PR:L/UI:N/S:C/C:H/I:L/A:L) — internal network access

---
## STATE UPDATE (exact format for parsing)
STATUS_PHASE: RECON
STATUS_STATE: IN_PROGRESS
NEXT_STEP_1: Enumerate live endpoints on TIER0 domains (HEAD /.well-known/*, /api/*, /_ah/*) to map attack surface
NEXT_STEP_2: Cross-reference CT subdomains with DNS resolution to identify dangling CNAMEs for takeover testing
NEXT_STEP_3: Deep-dive OT0 repo GitHub Actions configs for supply chain risks (unpinned actions, workflow_dispatch permissions, secret handling)
