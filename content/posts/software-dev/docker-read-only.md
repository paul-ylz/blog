---
title: 'Docker Read Only'
date: 2019-05-19T18:51:34+08:00
draft: false
---

Recently on a new project, our infra team had temporarily relaxed the security rules on our Kubernetes cluster to allow us to get things going quicker - a strategy I would vote against in the future. The new security rules would require our containers to run as unprivileged user (versus running as root, which is the docker default), as well as run with a read only root filesystem. It is straight forward enough to run containers as non root user... we just need a line to `RUN addusr`, include the `--chown appuser:appuser` with all the `COPY` directives in our dockerfile, and finish nicely with `USER appuser` or something like that. Getting around the read-only filesystem constraints are a bit trickier, as typically the framework is doing things for us which are not obvious to use in day-to-day development.

In our work setup we have a bunch of microservices running dotnet core, and one running node. None of our containers ran into any issues running as the non-root user. However, all of them had niggles running on a read only filesystem. The dotnet containers gave us a cryptic `Failed to initialize CoreCLR, HRESULT: 0x80004005`. A little bit of googling revealed that this had to do with dotnet trying to initialize a debugging tool which required access to write to a tmp directory. Disabling this tool needed an environment variable to be set, ala `ENV COMPlus_EnableDiagnostics 0`. Easy win! Many thanks.

It took a bit longer to figure out the node container, which had an ENTRYPOINT of `["yarn", "start"]`. The start script simply ran `node server.js`. The logged error complained that yarn did not have access to the cache directory and that yarn should be run with the --cache-folder parameter. This was a little misleading and we ended up trying many permutations of this parameter in conjunction with --pure-lockfile and --frozen-lockfile. All a bit of a wild goose chase. In the end, I learnt that yarn runs all things with the use of a cache, hence it's really not feasible to launch a process on a RO-FS using yarn. The seemingly OK solution was just to change the ENTRYPOINT to ["node", "server.js"].

Being that the feedback loop of trying this out on the CI is about 30 minutes, I spent a good half of a day figuring this out. Later in the night I realized how much time I could have saved if I had known that `docker run` accepted a `--read-only` parameter, which could be used to reproduce the issue as experienced on CI. Ahh!
