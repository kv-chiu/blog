---
title: Use environment variables in Hugo configuration
linkTitle: Use environment variables in Hugo configuration
date: 2025-11-05T01:39:00
author: "[kvchiu](https://kvchiu.pages.dev)"
summary: Besides hard-coding in config, environment variables are another way to set Hugo configuration.
draft: false
tags:
  - hugo
  - environment variables
  - configuration
---
# Use environment variables in Hugo configuration

Blowfish is a theme for Hugo. For site analytics, Blowfish has built‑in support for Fathom Analytics, Google Analytics, and Umami Analytics.

While configuring it, I ran into some confusion at first.

Taking Umami as an example:

![umami-configuration-docs.png](umami-configuration-docs.png)

The example in the official docs hard‑codes values in the configuration file.

That felt a bit uncomfortable. Even though I know these settings ultimately become part of the client and visitors can see the injected Umami script and my config values via the browser’s dev tools, I still didn’t want to hard‑code them in the file.

I started looking for a way to use environment variables in the configuration file.

The AI answers were quite hallucinatory: both gemini2.5‑pro and gpt5 assumed Hugo’s [getenv](https://gohugo.io/functions/os/getenv/) function used in templates could be used inside configuration files. It can’t. I also tried a few TOML tricks for variables, but none worked.

Still, I believed Hugo must support environment variables.

After some searching, I finally found hints in the community:

https://github.com/gohugoio/hugo/issues/6276

https://gohugobrasil.netlify.app/getting-started/configuration/#environmental-variables

That confirmed it: Hugo can use environment variables for configuration, so we don’t have to hard‑code values into config files.

Oddly, I couldn’t find an official mention easily. Using Context7 to read the official docs quickly yielded the answer:

![hugo-configuration-env-docs.png](hugo-configuration-env-docs.png)

https://gohugo.io/configuration/introduction/#environment-variables

Question settled: Hugo can indeed use environment variables for configuration.

Notes:
- Environment variable names must be all uppercase.
- For params, append PARAMS after the HUGO prefix.
- You can use `_` or `x` as separators; if a variable name contains `_`, you must use `x`.

# Further reading: about “configuration”

Using environment variables directly for configuration is a widely accepted practice in the industry.

The Twelve‑Factor App explicitly states:

![12factor-config.png](12factor-config.png)

In Hugo, there’s also a standard configuration lookup order:
1. Command‑line flags
2. Environment variables
3. Config file (project root)
4. Config file (config directory)

For well‑known projects there’s a 99% chance environment variables work for configuration. If you can’t find official documentation, try AI + Context7 semantic search or look for related discussions in the community.
