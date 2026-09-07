# DevOps and Cloud Interview Theory Guide

This is an original, concise theory guide for the questions listed in `Udemy_DevOps_Cloud_Questions_Provided.md`. Use the scenario answers as interview frameworks: state the principle, describe the checks, then explain the remediation and prevention.

## Git

**4. Fork vs clone**  
A fork is a server-side copy of another repository in your account or organization, commonly used to contribute without direct write access. A clone is a local working copy of a repository; you can clone your own repository, an upstream repository, or your fork.

**5. When use a fork instead of a clone?**  
Use a fork when contributing to a repository you do not control. Clone the fork locally, create a branch, push to the fork, and open a pull request to the upstream repository.

**6. Fork workflow**  
Fork the repository, clone the fork, add the original repository as `upstream`, create a feature branch, commit and push, then open a pull request. Keep the fork synchronized with `git fetch upstream` and a merge or rebase.

**7. Fetch vs pull**  
`git fetch` downloads remote commits and updates remote-tracking branches without changing the current branch. `git pull` fetches and then integrates the remote branch, usually by merge or rebase.

**8. Practical fetch and pull flow**  
After another user pushes, `git fetch origin` makes `origin/main` reflect the remote while the working branch remains unchanged. `git pull --rebase origin main` fetches and replays local commits on the updated remote branch.

**9. Which do you prefer?**  
Prefer `fetch` when inspecting changes before integration because it is safer and explicit. Use `pull --rebase` or `pull` when the integration policy is known and you are ready to update the working branch.

**10-12. Rebase vs merge**  
Merge creates a merge commit and preserves the exact branching history. Rebase moves commits onto a new base and creates a linear history, but rewrites commit IDs. Never rebase shared commits unless the team explicitly agrees; rebase private feature branches, resolve conflicts, test, and force-push with `--force-with-lease` when required.

**13. Branching strategy**  
Describe the strategy actually used: for example, protected `main`, short-lived feature branches, pull requests with review and CI, release branches only when needed, and tagged production releases. Explain branch protection, approvals, and hotfix handling rather than naming a strategy alone.

**14-15. Git challenges**  
Good examples include merge conflicts, accidental large or secret files, divergent branches, broken histories, or release-branch drift. Explain impact, investigation, the smallest safe fix, communication, and prevention such as hooks, secret scanning, branch protection, or clearer branching rules.

**16. Merge conflicts**  
Fetch first, identify the conflicting files, understand both versions, edit deliberately, remove conflict markers, run tests, `git add` the resolved files, and complete the merge or rebase. Never resolve a conflict by blindly choosing one side.

**17. Ours and theirs**  
During a merge, `ours` is the current checked-out branch and `theirs` is the branch being merged. During a rebase, the labels can feel counterintuitive because the commit being replayed is treated as the incoming change. Verify with `git status` and inspect the diff before choosing.

**18. Git tags**  
Tags are stable names for commits, commonly release versions such as `v2.1.0`. Annotated tags store metadata and are preferred for releases; tags should be immutable by policy and signed when provenance matters.

**19. Combine commits**  
Use interactive rebase, for example `git rebase -i HEAD~3`, and change later `pick` entries to `squash` or `fixup`. Do this only for unpublished or privately owned history, then run tests and push safely.

**20. Ten daily Git commands**  
Typical commands are `status`, `clone`, `fetch`, `pull`, `switch`, `branch`, `add`, `commit`, `push`, `log`, `diff`, `merge`, `rebase`, `restore`, and `cherry-pick`. Explain what you use each for rather than listing commands without context.

**21. Ignore changes to a tracked file**  
`.gitignore` affects untracked files, not files already tracked. For a local-only change to a tracked file, use `git update-index --skip-worktree file` cautiously; a better long-term design is configuration outside the repository or a checked-in template.

**22. `.git` folder**  
It contains repository metadata: object database, refs, HEAD, index, configuration, hooks, and logs. Deleting it removes local Git history and configuration while leaving ordinary working files.

**23. Restore a deleted `.git` folder**  
A clone or backup is the safest restoration. Without one, initialize a new repository and add the files, but commit history, branches, remotes, and reflogs cannot be reconstructed automatically.

**24. Secret committed to Git**  
Treat the secret as compromised: revoke or rotate it immediately, remove it from the current tree, and clean history with an approved tool such as `git filter-repo` or BFG. Force-push only with coordination, update consumers, and add secret scanning and external secret storage. Base64 is encoding, not encryption.

## Linux

**25. Ten daily Linux commands**  
Use `pwd`, `ls`, `cd`, `cp`, `mv`, `rm`, `mkdir`, `cat`, `less`, `grep`, `find`, `awk`, `sed`, `tail`, `ps`, `top`, `df`, `du`, `free`, `ss`, `systemctl`, and `journalctl`. Mention permissions and safe use of `rm` in production.

**26. Lost PEM file**  
The private key is not recoverable from a normal EC2 instance. Use an existing authorized key, Session Manager, another administrative path, cloud rescue workflow, or replace the key through a controlled recovery process; do not expose or recreate the old private key.

**27. `/var` nearly full**  
Use `df -h` and `df -i`, then `du -xhd1 /var` to locate space. Inspect logs, journal retention, package caches, deleted-but-open files with `lsof +L1`, and unexpected data; rotate or archive safely, expand storage if necessary, and add alerts.

**28. High CPU**  
Confirm scope with `top`, `htop`, `ps`, load average, and per-process metrics. Identify whether CPU is application, kernel, I/O wait, or container related; inspect logs and recent changes, throttle or restart only if safe, fix the root cause, and scale or tune capacity.

**29. Nginx connection refused**  
Check `systemctl status nginx`, `journalctl`, configuration with `nginx -t`, listeners with `ss -lntp`, local curl, firewall/security groups, upstream health, and DNS. Confirm Nginx is bound to the expected interface and the upstream application is listening on the expected port.

**30. SSH stopped working**  
Check instance health, route and security group/NACL rules, public IP or bastion path, port 22 reachability, `sshd` status and logs, disk space, CPU, key and username, file permissions, and host firewall. Use console or Session Manager for out-of-band recovery.

**31-32. Find and remove old logs**  
First list safely: `find /var/log -type f -mtime +7 -print`. For deletion, review ownership and paths, then use a narrowly scoped command such as `find /path -type f -mtime +30 -print -delete`; prefer logrotate and archiving for managed logs.

**33. Advanced log rotation**  
Use logrotate where possible: define rotation frequency, retention, compression, `copytruncate` only when reopening is impossible, or `postrotate` to signal the service. A script should use strict mode, absolute paths, locking, validation, permissions, dry-run testing, and cron logging.

**34. Bulk users from CSV**  
Validate the CSV, reject duplicates and unsafe usernames, create users with `useradd`, set groups and home directories, disable password login where appropriate, and log results. Never place plaintext passwords in a CSV; use secure provisioning and forced first-login rotation.

**35. Bash health monitor**  
Probe the service locally with a timeout, check HTTP status or systemd state, log structured results, and alert only after a retry threshold. Run with least privilege, quote variables, use absolute commands, and make the script idempotent.

**36. Delete files over 100MB**  
Discover first with `find /path -type f -size +100M -print` and inspect each candidate. Delete only with an explicit approved path, or archive/compress according to retention policy; do not blindly delete database or application data.

**37. Users logged in today**  
Use `last` for login history and `who` or `w` for current sessions. Correlate timestamps with auth logs such as `/var/log/auth.log` or `journalctl`, remembering that package deletion or tampering may require integrity and audit-log checks.

**38. Website does not load**  
Follow the path: DNS resolution, TCP reachability, TLS, load balancer, web server, upstream application, database/dependencies, and client/browser. Compare external and localhost curl results, inspect status codes and logs, and check recent deployments, certificates, resource saturation, and firewall rules.

**39. Remove first and last line with `sed`**  
For a stream, `sed '1d;$d' file` deletes the first and last input lines. Write to a temporary file and atomically replace the original if editing in place is required; preserve a backup for important files.

**40. Linux variable types**  
Shell variables are strings by default, with positional parameters, special variables, environment variables, and arrays. A variable becomes exported to child processes with `export`; scope can be local to a function or global to the shell.

**41. `kill` vs `kill -9`**  
`kill` normally sends SIGTERM, allowing cleanup and graceful shutdown. `kill -9` sends SIGKILL, which cannot be caught or cleaned up, so use it only after diagnosing why graceful termination failed.

## Networking

**42. DNS**  
DNS maps names to records such as A/AAAA addresses, CNAME aliases, MX mail servers, and TXT data. A resolver checks cache, asks authoritative name servers through the DNS hierarchy, and returns a result subject to TTL.

**43. Request flow and OSI**  
The client resolves DNS, creates a TCP connection, negotiates TLS for HTTPS, sends an HTTP request, and traverses routing, firewalls, load balancers, and the server stack. In OSI terms, data is encapsulated from application through transport, network, data-link, and physical layers, then decapsulated on return.

**44. Forward vs reverse proxy**  
A forward proxy represents clients to external services and can enforce egress policy or anonymity. A reverse proxy represents servers to clients and commonly provides TLS termination, routing, caching, load balancing, and WAF functions.

**45. Application slowness**  
Define the affected path and time window, compare latency, error rate, throughput, and saturation, then trace from client through DNS, network, proxy, application, database, and dependencies. Use metrics, logs, traces, recent-change data, and a controlled rollback or scale action.

**46. Curl IP works but domain fails**  
Likely causes are DNS resolution, wrong record, split-horizon DNS, IPv6 preference, SNI/Host-header routing, certificate mismatch, or proxy behavior. Compare `dig`, `curl -v`, `curl --resolve`, IPv4/IPv6 tests, and the server's virtual-host configuration.

**47. HTTP 502**  
A gateway or proxy could not obtain a valid response from its upstream. Check proxy and upstream logs, service health, port and protocol, DNS, timeouts, TLS, connection limits, and resource saturation; test the upstream directly.

**48. `0.0.0.0` vs `127.0.0.1`**  
`127.0.0.1` is the local loopback address and is reachable only from the same host. `0.0.0.0` is the IPv4 wildcard for binding, meaning all local interfaces; as a destination it is not a normal remote host address.

**49. Public vs private subnet**  
A public subnet has a route to an Internet Gateway and resources need public addressing and allowed security rules to be internet reachable. A private subnet has no direct Internet Gateway route; outbound access commonly uses NAT, while inbound access comes through controlled internal or edge components.

**50. Fix a private subnet created instead of public**  
Add or correct the route table association and route `0.0.0.0/0` to an Internet Gateway, ensure the instance has a public IPv4 or load balancer path, and check security groups/NACLs. Do not expose a sensitive workload merely to fix a label; validate the intended architecture first.

## CI/CD

**51. Jenkins shared libraries**  
A shared library is versioned Groovy code containing reusable pipeline steps, vars, and classes. Configure it globally or per job, pin a trusted version, keep privileged code reviewed, and call standard steps from a small Jenkinsfile.

**52. Five build targets**  
Typical targets are clean, compile/package, unit test, integration test, static analysis, security scan, container image build, publish, and deploy. Maven targets include `clean`, `compile`, `test`, `package`, `verify`, and `deploy`.

**53. Artifact repository**  
An artifact repository stores immutable, versioned build outputs and proxies upstream dependencies. Examples are JFrog Artifactory, Nexus, GitHub Packages, and AWS CodeArtifact; choose based on ecosystem, access control, retention, availability, and audit needs.

**54. Artifactory with Maven**  
Configure repository URLs and credentials in Maven `settings.xml` using a server ID, and declare repositories or distribution management in `pom.xml`. Use CI-injected credentials, TLS, least privilege, immutable versions, and avoid committing secrets.

**55. Local pass, CI failure**  
Compare OS, JDK/Python/tool versions, environment variables, dependency lockfiles, working directory, permissions, network, caches, and test data. Reproduce in the CI container, inspect the first meaningful error, remove hidden local state, and pin the toolchain.

**56. CI succeeds, production is broken**  
Start incident response: assess impact, stop further rollout, use health checks and logs, rollback or roll forward safely, and communicate. Then identify the gap in tests, configuration, migrations, observability, or deployment validation and add a prevention control.

**57. Pipeline slows over time**  
Measure stage durations and queue time, compare historical trends, and inspect dependency downloads, cache hit rate, test growth, agents, parallelism, artifact retention, and external services. Cache safely, parallelize independent work, shard tests, clean workspaces, and scale agents.

**58. Feature branch pipeline not triggered**  
Check webhook delivery, branch filters, multibranch discovery, job permissions, SCM credentials, YAML/Jenkinsfile path, trigger rules, and whether the commit actually reached the remote. Test webhook payload handling and inspect the CI event log.

**59. Dependency cannot download**  
Check repository URL, DNS/TLS, credentials, proxy, repository availability, artifact coordinates and version, permissions, and whether the dependency is in the correct virtual repository. Use a lockfile or internal mirror and do not bypass verification or publish mutable artifacts.

**60. Python works locally, fails in CI**  
Common causes are Python version, missing system package, unpinned dependency, virtual environment differences, case-sensitive paths, missing environment variables, network restrictions, locale, or filesystem permissions. Build from a clean pinned environment and inspect the full traceback.

**61. Python application build**  
Create a clean virtual environment, install from a pinned `requirements.txt` or `pyproject.toml` lock, run lint/type/unit/integration tests, package the application or image, scan dependencies, publish an immutable artifact, and deploy with configuration supplied at runtime.

**62. Static analysis finds**  
It can identify bugs, code smells, unreachable code, insecure APIs, injection risks, secrets, dependency vulnerabilities, complexity, duplication, and style violations. Rules are indicators, not proof; triage false positives and enforce quality gates proportionately.

**63. Static analysis slows CI**  
Profile the scanner, cache dependencies, scan only changed code where valid, run stages in parallel, use incremental analysis, tune rules, and move deep scans to scheduled pipelines. Keep security-critical checks blocking and make the tradeoff visible.

**64. Argo CD OutOfSync with no Git change**  
Compare live and desired manifests with `argocd app diff`; inspect controller logs, generated Helm/Kustomize output, ignored fields, defaulting, mutating webhooks, and operators that continuously modify resources. Configure intentional `ignoreDifferences` carefully rather than hiding drift.

**65. Jenkins failure email**  
Use a post-build `failure` action or `post { failure { ... } }` with the Email Extension plugin, SMTP configuration, recipients, subject, and a useful log/build URL. Avoid noisy alerts by notifying owners or a team channel and testing SMTP securely.

## Terraform

**66. `for_each` vs `for`**  
`for_each` creates multiple resource instances keyed by a map or set, giving stable addresses such as `resource.name["key"]`. A `for` expression transforms a collection into a list, map, or set value; it does not itself create resource instances.

**67. Modules**  
A module is a reusable group of Terraform resources with inputs, outputs, and versioning. Modules standardize patterns and ownership, but should have clear interfaces, documented assumptions, tests, and pinned versions.

**68. Statefile role**  
State maps Terraform configuration addresses to real provider objects and stores attributes needed to calculate changes. It is sensitive operational data, not ordinary source code; protect it with encryption, access control, versioning, and locking.

**69. State in Git?**  
Avoid it because state can contain secrets, causes merge conflicts, and cannot provide reliable concurrent locking. Prefer a remote backend such as S3 with encryption/versioning/locking, Azure Blob with locking, or a managed Terraform service.

**70. State management**  
Use one backend per environment or controlled workspace, enable encryption, versioning and locking, restrict access, back up state, review plans, and use `terraform state` commands only deliberately. Never edit JSON manually unless performing a documented recovery.

**71. Two engineers update state simultaneously**  
A locking backend allows one operation and makes the other wait or fail, preventing corruption. Without locking, concurrent writes can race and lose state updates; use a backend with locking and a team workflow.

**72. No cloud account for state**  
Use a local state only for isolated learning, or a managed/hosted backend, Terraform Cloud, an organization-approved object store, or another backend supported by Terraform/OpenTofu. Do not put shared state in Git.

**73. Terraform Enterprise vs Community**  
Community/OpenTofu CLI is flexible and self-managed. Terraform Cloud/Enterprise adds remote runs, policy, private registry, team permissions, audit, and state management. Choose based on governance, scale, compliance, and operational ownership.

**74. OpenTofu vs Terraform**  
OpenTofu is an open-source Terraform-compatible fork under the Linux Foundation; Terraform is HashiCorp's product with its own licensing and ecosystem. Compare provider/module compatibility, required features, governance, support, and migration risk rather than claiming one is universally better.

**75. Terraform AWS resource**  
A minimal example is an AWS provider plus an `aws_s3_bucket` resource, with variables and outputs. Production code should use provider version constraints, encryption, public-access blocks, tags, a remote backend, and a reviewed plan; never hardcode credentials.

**76. Resource vs data source**  
A resource creates or manages an object and changes it during apply. A data source reads an existing object or external information and exposes it to configuration; it does not manage that object's lifecycle.

## Docker

**77. Container exits immediately**  
A container lives as long as its PID 1 process. Inspect `docker ps -a`, `docker logs`, exit code, image command, environment, mounts, and entrypoint; run an interactive shell or override the command to inspect the image.

**78. `EXPOSE`**  
`EXPOSE` documents the port an image expects to use; it does not publish the port or create firewall rules. Publish explicitly with `-p host:container` or an orchestration Service.

**79. Port mapping fails**  
Confirm the container is running and the application listens on `0.0.0.0`, not only `127.0.0.1` inside the container. Check `docker port`, published port collisions, protocol, host firewall, container logs, and test from the host.

**80. Container data disappears**  
The writable container layer is ephemeral. Use named volumes, bind mounts, or external durable storage, and back up data according to the application consistency model.

**81. Code change not reflected**  
Confirm the new file is in the build context, not excluded by `.dockerignore`, rebuild with the correct tag and context, inspect image layers, remove stale containers, and run the new image. Use `--no-cache` only to diagnose caching, not as the default fix.

**82. Permission denied in container**  
Compare UID/GID, file ownership, mount permissions, read-only filesystems, security profiles, and the container user with the host-mounted files. Prefer a non-root image user and fix ownership at build/provision time rather than running everything as root.

**83. Docker disk cleanup**  
Measure with `docker system df`, then remove stopped containers, unused images, dangling layers, unused networks, and unused volumes only after checking dependencies. Configure image retention and log rotation; do not delete a volume containing needed data.

**84. Debug live container**  
Use `docker exec`, `docker logs`, `docker inspect`, `docker top`, resource stats, and network inspection. Capture evidence first, avoid changing production state unnecessarily, and reproduce in a controlled environment.

**85. Container registry**  
Choose a registry such as ECR, ACR, GCR/Artifact Registry, Docker Hub, or an internal registry based on cloud integration, scanning, replication, access control, retention, and latency. Use immutable tags or digests and scan images.

**86. `CMD` vs `ENTRYPOINT`**  
`ENTRYPOINT` defines the main executable and is harder to override; `CMD` supplies default arguments or a default command. Exec-form JSON syntax handles signals correctly; combine a stable entrypoint with configurable default arguments.

**87. Daily Docker commands**  
Use `build`, `run`, `ps`, `logs`, `exec`, `inspect`, `pull`, `push`, `images`, `tag`, `stop`, `rm`, `rmi`, `volume`, `network`, and `system df`. Mention safe cleanup and image digest verification.

**88. Force-remove a container**  
Use normal `docker stop` first so the process can handle SIGTERM. Use `docker rm -f` only when it is stuck or graceful termination is impossible, after checking whether data or dependent services are affected.

## Kubernetes

**89. Cluster architecture**  
The control plane includes the API server, etcd, scheduler, and controller managers. Worker nodes run kubelet, a container runtime, and networking components such as kube-proxy or an eBPF implementation; desired state is stored in the API/etcd and controllers reconcile it.

**90. `kubectl apply` flow**  
The client authenticates to the API server, which validates and stores desired state in etcd. Controllers create or update dependent objects, the scheduler assigns unscheduled Pods to nodes, kubelet pulls images and starts containers, and status is reported back through the API.

**91. Services**  
A Service gives a stable virtual IP and DNS name for a changing set of Pods selected by labels. It decouples clients from Pod IPs and can provide internal or external exposure.

**92. Pod IP hardcoding**  
Pod IPs change during restart, rescheduling, scaling, and deployment. Use a Service, DNS, and appropriate discovery mechanism instead.

**93. Service types**  
`ClusterIP` is internal, `NodePort` exposes a port on nodes, `LoadBalancer` integrates with an external load balancer, and `ExternalName` maps to an external DNS name. Ingress is a separate HTTP routing API, not a Service type.

**94. Labels and selectors**  
Labels are key-value metadata; selectors choose objects by labels. Deployments, Services, and policies depend on selectors, so changing labels carelessly can disconnect traffic or ownership.

**95. NodePort vs LoadBalancer**  
Use LoadBalancer for supported cloud-managed external access because it offers a stable edge integration. Use NodePort mainly for development, on-premises designs with an external load balancer, or when direct node-port exposure is intentional.

**96. Services and kube-proxy**  
The Service API defines a virtual endpoint and EndpointSlices identify backends. kube-proxy programs node networking rules, traditionally iptables/IPVS, to translate Service traffic to Pod endpoints; some distributions replace this with eBPF.

**97. LoadBalancer disadvantage**  
It can create cloud cost and provider-specific resources, has quotas and provisioning delays, and may expose a separate load balancer per Service. Use shared ingress or gateway patterns when suitable.

**98. Headless Service**  
A headless Service sets `clusterIP: None`, so DNS returns Pod endpoints instead of a virtual load-balanced IP. It is useful for StatefulSets and client-side discovery where individual replicas matter.

**99. Cross-namespace Service access**  
Yes, use the DNS name `service.namespace.svc.cluster.local` and ensure NetworkPolicies and ports permit it. Service names are namespace-scoped, so the namespace must be explicit.

**100. Restrict database access to one app**  
Use a NetworkPolicy selecting the database Pods and allow ingress only from Pods with the application's labels in the application namespace. Also enforce database authentication, TLS, and egress controls; policies require a supporting CNI.

**101-104. Deployment strategies**  
Rolling updates replace Pods gradually; blue-green switches traffic between complete environments; canary sends a small percentage to the new version; recreate causes downtime but is simple. Select based on compatibility, rollback speed, capacity, database migrations, and observability.

**102. Rollback strategy**  
Keep immutable versions and use Deployment revision history or GitOps revert to restore the last known-good version. Verify health, database compatibility, traffic, and data integrity; rollback application code cannot automatically undo an irreversible schema migration.

**103. Avoid rollbacks**  
Use progressive delivery, automated tests, contract testing, feature flags, backward-compatible migrations, health checks, realistic staging, canaries, and automatic analysis. The goal is early detection and safe forward fixes, not pretending failures cannot happen.

**105. CoreDNS**  
CoreDNS serves cluster DNS, resolving Service and Pod records such as `service.namespace.svc.cluster.local`. It uses the Kubernetes plugin to watch API objects and can forward external queries; inspect CoreDNS Pods, ConfigMap, Service, and network policy when DNS fails.

**106. Tainted node**  
A `NoSchedule` taint prevents new Pods without a matching toleration from being scheduled. Existing Pods are not necessarily evicted; `NoExecute` affects existing Pods. A toleration permits scheduling but does not force placement without affinity or selectors.

**107. CrashLoopBackOff**  
Inspect `kubectl describe pod`, current and previous logs, exit code, events, command, probes, environment, mounts, permissions, dependencies, and resource limits. Reproduce with the same image/configuration, fix the cause, and redeploy an immutable version.

**108. Liveness vs readiness**  
Readiness controls whether a Pod receives traffic. Liveness determines whether kubelet should restart it. A startup probe protects slow-starting applications; probes should test meaningful health without causing restart loops.

**109. Ingress vs LoadBalancer**  
A LoadBalancer Service generally exposes one Service through an external load balancer. Ingress provides Layer 7 HTTP/HTTPS host/path routing and TLS across multiple Services through an Ingress controller.

**110-112. Ingress troubleshooting and custom load balancers**  
Check Ingress class/controller, events, generated proxy configuration, DNS, TLS secret, rules, Service selector/endpoints, target port, NetworkPolicy, and backend logs. An in-house load balancer can work if it routes to the controller's exposed address and supports the required protocol; the Ingress resource alone does nothing without a controller.

**113. Replicas 3 but one Pod**  
Check Deployment and ReplicaSet events, `kubectl describe`, scheduling failures, resource requests, quotas, node capacity, taints, affinity, image pull errors, probes, and admission policies. The desired replica count is not proof that scheduling and startup succeeded.

**114. ConfigMap changes not reflected**  
Mounted ConfigMap files are eventually updated, but applications do not automatically reload them; `subPath` mounts do not receive updates. Restart or reload the application, use a checksum annotation for controlled rollouts, and never store secrets in a ConfigMap.

**115. Node Affinity**  
Node affinity schedules Pods onto nodes with matching labels. `requiredDuringScheduling` is hard, while `preferredDuringScheduling` is a weighted preference; use it for hardware, zones, licensing, or workload placement and combine it with anti-affinity when needed.

**116. Node affinity vs node label selector**  
A node selector is a simple hard equality constraint. Node affinity is more expressive, supports operators, required/preferred rules, and combinations; both rely on trusted node labels.

**117. Container runtime**  
The runtime pulls images and creates, starts, stops, and isolates containers using the Kubernetes CRI. Common runtimes include containerd and CRI-O; kubelet communicates through CRI rather than directly managing image processes.

**118. Kubernetes QoS**  
Guaranteed Pods have requests equal to limits for every container; Burstable Pods have some resource settings; BestEffort have neither. Under resource pressure, QoS influences eviction priority, with BestEffort generally most vulnerable.

**119. Requests and limits**  
Requests influence scheduling and reserve a baseline; limits cap usage. CPU limits can throttle, memory overuse can cause OOM kills, and poor values cause waste or instability, so set them from measurement and workload behavior.

**120. Kubernetes challenges**  
Strong examples are networking/debugging, resource tuning, secrets and policy, upgrades, observability, or deployment safety. Explain symptoms, commands and evidence, remediation, and the preventive automation or documentation added.

**121. Schedule on control-plane node?**  
Usually control-plane nodes are tainted to keep workloads off them. It is technically possible by removing the taint or adding a toleration, but production clusters should preserve control-plane capacity unless the design explicitly requires otherwise.

**122. Horizontal vs vertical scaling**  
Horizontal scaling adds replicas or nodes and improves availability, but requires stateless design and coordination. Vertical scaling gives a resource more CPU or memory and is simpler but has hardware limits and often requires restart or replacement.

**123. Kubernetes Secrets**  
Secret types include opaque data, service-account tokens, Docker registry credentials, TLS certificates, and bootstrap tokens. Kubernetes Secrets are base64-encoded and require encryption at rest, RBAC, limited exposure, rotation, and preferably an external secret manager.

## Observability

**124. Monitoring vs observability**  
Monitoring tells you whether known conditions are healthy through predefined signals and alerts. Observability uses logs, metrics, traces, and context to explain unknown internal states and why a system behaves as it does.

**125. Custom logs and metrics**  
Use structured JSON logs with correlation IDs and appropriate levels, and expose application metrics such as counters, gauges, and histograms on a protected metrics endpoint. Avoid secrets, high-cardinality labels, and unbounded log volume.

**126. Prometheus metrics**  
Scrape golden signals: request rate, errors, latency, saturation, plus JVM/runtime, container, node, Kubernetes, database, and business metrics. Keep labels bounded and define recording rules for common queries.

**127. Explain observability experience**  
Describe instrumentation, collection, dashboards, alert rules, SLOs, incident use, and improvements. Tie the work to reduced detection or resolution time rather than listing tools only.

**128. Logs, metrics, traces**  
Logs are event records, metrics are aggregated numerical time series, and traces follow a request across components with spans. Metrics detect, traces localize, and logs explain detailed events.

**129. Push vs pull monitoring**  
In pull mode, the monitoring system scrapes targets and controls collection; Prometheus commonly works this way. Push mode sends data to a gateway or backend and suits short-lived jobs or restricted networks, but requires handling retries, identity, and stale data.

**130. Observability stack**  
A typical stack is Prometheus or AMP for metrics, Grafana or AMG for visualization, Alertmanager for routing, Fluent Bit/OpenTelemetry for logs, and OpenTelemetry plus Tempo/Jaeger/X-Ray for traces. Choose based on scale, retention, integrations, and operational cost.

**131. Slow app, no errors, CPU good**  
Check latency percentiles, saturation of memory/disk/network/thread pools, database query latency and locks, downstream dependencies, connection pools, GC, queue depth, and traces. Averages and CPU alone cannot prove health.

**132. Trace across microservices**  
Propagate W3C trace context through HTTP or messaging, instrument ingress and each service with OpenTelemetry, export spans to a collector/backend, and correlate trace IDs with structured logs and metrics.

**133. OOMKilled**  
Confirm with Pod status, events, previous logs, container and node memory metrics, and limit/request values. Identify leaks, spikes, cache behavior, or incorrect limits; fix code/configuration, tune resources, and add alerts before increasing limits blindly.

**134. Reduce false alarms**  
Review alert history, remove duplicates, alert on user-impacting symptoms and SLO burn rates, add duration/for conditions and sensible thresholds, group and route notifications, and test alerts regularly. Every page should have a clear action.

## AWS

**135. Highly available multi-tier app**  
Use multiple Availability Zones: public load balancer, private application subnets with autoscaling, private database subnets with Multi-AZ, least-privilege IAM, encrypted storage, backups, health checks, monitoring, and infrastructure as code. Add caching, queues, and multi-region only when requirements justify them.

**136. NAT**  
A NAT Gateway lets private-subnet resources initiate outbound IPv4 connections without accepting unsolicited inbound Internet traffic. Place one per AZ for resilience, route private subnets to it, and consider cost and IPv6 alternatives.

**137. Internet access from a private subnet**  
For outbound access, route through a NAT Gateway in a public subnet whose route table points to an Internet Gateway. For inbound application access, use a public load balancer that forwards to private targets; do not give the application direct public exposure unnecessarily.

**138. VPC subnet interaction**  
Subnets in the same VPC can route to one another through the implicit local route, but security groups, NACLs, host firewalls, routes, and application listeners still control whether traffic succeeds.

**139. NACL vs security group**  
Security groups are stateful, resource-level allow lists. NACLs are stateless, subnet-level rule lists with allow and deny entries. Use security groups as the primary control and NACLs for coarse subnet boundaries or explicit deny requirements.

**140. EC2 terminated unexpectedly**  
Check CloudTrail, EC2 state-transition events, Auto Scaling activity, scheduled events, instance/system logs, and automation or operator actions. Determine whether termination protection, ASG desired capacity, spot interruption, or instance failure is involved; restore from immutable images and backups.

**141. Random Lambda failures**  
Inspect CloudWatch logs, metrics, request IDs, duration, throttles, concurrency, timeouts, memory, initialization, downstream errors, VPC networking, permissions, and payload limits. Add retries with idempotency and a dead-letter destination where appropriate.

**142. RDS storage full**  
Confirm free storage and growth rate, identify logs, temporary tables, indexes, and data retention, and enable storage autoscaling or scale the instance/storage safely. Remove data only under policy, monitor replication and maintenance impact, and fix the growth source.

**143. Deleted critical AWS resources**  
Declare an incident, preserve evidence, identify the actor in CloudTrail, stop further destructive automation, restore from backups/versioning/IaC, and validate dependencies. Apply least privilege, MFA, approvals, deletion protection, SCPs, retention, and tested recovery procedures.

**144. Cost optimization**  
Use cost allocation tags, Cost Explorer, budgets, and usage metrics to remove idle resources, right-size compute, use autoscaling and schedules, optimize storage tiers, purchase commitments where stable, and reduce data-transfer waste without violating reliability targets.

**145. AWS challenge**  
Use a STAR answer: situation and impact, technical investigation, decision and tradeoff, implementation, validation, and prevention. Include concrete evidence such as metrics, timeline, reliability or cost outcome, and what you learned.

**146. ASG not launching EC2**  
Check ASG activity history, desired/min/max capacity, launch template version, AMI, subnet capacity, AZ health, instance profile, security groups, quotas, limits, purchase option, and health-check failures. Read the exact failure reason before changing settings.

**147. Daily AWS services**  
Explain services you genuinely use, such as IAM, VPC, EC2, S3, ECR, EKS, RDS, Route 53, ALB, CloudWatch, Secrets Manager, and Lambda. Connect each to an operational responsibility instead of reciting a catalog.

**148. EFS experience and issues**  
EFS is shared, elastic, network file storage accessed through mount targets. Common issues are security groups, mount target/AZ design, DNS, throughput mode, latency, permissions, stale mounts, and cost; use encryption, access points, and monitoring.

**149. EFS over EBS**  
Choose EFS when multiple instances or AZs need a shared POSIX filesystem and elastic capacity. Choose EBS for a single instance or database requiring predictable low-latency block storage; compare performance, availability, cost, and access semantics.

**150. Disable console access**  
Remove or disable the IAM user's console password and prefer federation/SSO. Preserve required programmatic access only through separate roles or credentials, review active keys, and apply least privilege and MFA.

**151. Cross-account Lambda to S3**  
Use a role assumed by Lambda in Account A and a bucket policy in Account B allowing that role's ARN, with identity policy, trust policy, KMS permissions if encrypted, and correct region/network access. Avoid long-lived access keys.

**152. STS**  
AWS Security Token Service issues temporary credentials through operations such as `AssumeRole`, with access key, secret, session token, and expiration. Temporary credentials support federation, cross-account access, and least privilege.

**153. Trust policy**  
A role trust policy defines who or what may assume the role; it is the role's resource-based policy. The assuming principal also needs an identity permission, and conditions should restrict account, external ID, source identity, or session context as appropriate.

**154. Cross-account Lambda to DynamoDB**  
Create a role in Account B with DynamoDB permissions and a trust policy allowing a Lambda execution role from Account A. Allow the Account A role to call `sts:AssumeRole`, use the temporary credentials, and grant KMS permissions if needed.

**155. EBS in Kubernetes disadvantages**  
EBS volumes are AZ-scoped, commonly single-node attached depending on mode, and can complicate rescheduling, backup, portability, and cost. They are appropriate for block-storage workloads, but shared or multi-AZ data may need EFS or a managed database.

**156. Secrets Manager vs Parameter Store**  
Secrets Manager is designed for secrets with rotation workflows, richer secret features, and higher cost. Parameter Store offers hierarchical configuration and secure strings, with simpler and often cheaper storage; both require IAM, encryption, rotation policy, and audit logging.

**157. Database activities**  
Discuss backups and restore tests, monitoring, user/access management, schema migration support, performance analysis, storage/capacity, replication, patching, incident response, and automation. Avoid claiming DBA ownership if your role was platform support.

**158. Lambda activities**  
Describe triggers, packaging, IAM, environment configuration, VPC integration, timeouts/memory, deployment, observability, retries, idempotency, and cost. Explain one real automation or event-driven workflow end to end.

**159. IAM user vs role**  
A user is a long-lived identity, generally for a human or legacy application; a role is assumed and provides temporary credentials. Prefer federation and roles for people and workloads, with MFA, least privilege, and no shared users.

## Azure

**160. App Service random downtime**  
Check App Service health, platform events, availability, restarts, deployment slots, application logs, dependency latency, quotas, scaling, certificates, DNS, and regional incidents. Use health checks and a staging slot, then correlate downtime with deployments and metrics.

**161. Schedule a daily Azure script**  
Use Azure Automation, a Function Timer trigger, Logic Apps, Container Apps jobs, or an external scheduler. Store secrets in Key Vault or managed identity, configure timezone explicitly, make the job idempotent, and monitor failures.

**162. Cannot SSH/RDP to Azure VM**  
Check VM power/health, public/private path, NSG and route rules, firewall, correct username and credentials, SSH/RDP service, disk and CPU, DNS, Bastion or serial console, and boot diagnostics. Recover out of band rather than repeatedly opening broad ports.

**163. Azure Function failure**  
Inspect Application Insights, Function logs, invocation details, trigger configuration, host/runtime version, storage account, permissions, networking, timeout, memory, dependency, and configuration settings. Reproduce with a correlation ID and test a safe redeployment.

**164. Restrict storage to VNet VMs**  
Use storage firewall/network rules with selected virtual networks, private endpoints and private DNS, and restrict public network access. Combine managed identity/RBAC with storage data-plane roles; network restriction alone is not authorization.

**165. Production SQL to staging daily**  
Choose native backup/restore, replication, export/import, or a data pipeline based on RPO, size, downtime, and masking requirements. Encrypt transfer, mask sensitive data, automate and validate restores, and ensure staging credentials and network access are separate.

**166. Azure tags**  
Tags provide ownership, environment, cost center, data classification, and lifecycle metadata. Enforce required tags with Azure Policy, deny or modify effects where appropriate, and apply them through IaC so drift is corrected.

**167. Bicep vs ARM vs Terraform**  
Bicep is concise Azure-native IaC compiled to ARM; ARM JSON is the lower-level native format; Terraform is multi-cloud and uses state with a provider model. Choose based on cloud scope, team skills, policy, existing state, module ecosystem, and operational consistency.

**168. Prevent accidental deletion**  
Use RBAC least privilege, resource locks, management groups and policy, protected branches and approvals for IaC, backups, soft delete/versioning, and separate production subscriptions. Locks are a guardrail, not a replacement for authorization and recovery.

**169. Secure AKS app-to-app communication**  
Use Kubernetes NetworkPolicies, private cluster/network paths, namespace and workload identity boundaries, TLS or service mesh mTLS, least-privilege RBAC, secret management, and ingress/egress controls. Verify the CNI enforces the policy.

**170. Azure VM to on-prem database**  
Use site-to-site VPN or ExpressRoute, compatible VNet routing, DNS resolution, NSGs/firewalls, database allowlists, TLS, and private addressing. Test each layer and use managed identity or a secret vault for credentials.

**171. Multi-region redundancy and performance**  
Choose active-active or active-passive based on consistency and RTO/RPO, replicate data with a defined conflict model, use global routing and health probes, deploy in paired or suitable regions, and test failover. Measure cross-region latency and transfer cost.

**172. East US app slow in Europe**  
Measure user-to-region latency and identify whether the issue is compute, database, network, or content delivery. Use Front Door/CDN and caching, deploy a regional edge or replica, optimize database access, and validate with regional telemetry.

**173. Service Principal vs Managed Identity**  
Managed identity avoids storing client secrets and is preferred for Azure-hosted workloads. Service principals are useful for external systems or cross-platform automation but require credential rotation, secure storage, and least privilege.

**174. NSG and ASG**  
NSGs contain allow/deny rules for subnet or NIC traffic. Application Security Groups let rules refer to logical application groups instead of IP lists, improving maintainability; combine them with route controls and host/application security.

## Python

**175. Common DevOps packages**  
Examples include `boto3` for AWS, Azure SDK packages, `requests` or `httpx` for APIs, `PyYAML` for YAML, `click` or `argparse` for CLIs, `pytest` for tests, `paramiko` for SSH, and `prometheus_client` for metrics. Pin and scan dependencies.

**176. Python day-to-day task**  
Give a real automation example such as parsing deployment results, querying cloud resources, rotating configuration, validating manifests, or generating a report. Explain inputs, error handling, authentication, idempotency, tests, and operational output.

**177. Search a huge log file**  
Stream the file rather than loading it into memory: use a compiled regular expression and iterate line by line, printing matching line numbers or summaries. Handle encoding, malformed lines, rotation, permissions, and output volume; for simple searches, `grep` may be more efficient.

## Project Management and SDLC

**178. Typical workday**  
Structure the answer around priorities: review alerts and deployments, stand-up/planning, implementation or automation, code review, incident/support work, documentation, and learning. Emphasize prioritization, communication, and measurable outcomes.

**179. DevOps experience**  
Summarize years, environments, cloud, IaC, CI/CD, containers, Kubernetes, observability, security, and incident work. Then give one measurable example showing reliability, deployment speed, cost, or reduced manual effort.

**180-181. Contribution to the team**  
For a senior answer, discuss technical direction, mentoring, standards, incident leadership, stakeholder communication, and platform improvements. For a newer answer, emphasize reliable delivery, ownership, collaboration, documentation, learning, and gradually expanding responsibility.

**182. Current project**  
Explain the business goal, architecture, your responsibilities, delivery flow, security, observability, major challenge, solution, and result. Keep confidential details abstract and be clear about what you personally implemented versus what the team owned.

## Interview Answer Pattern

For scenario questions, use this sequence:

1. Clarify impact, scope, timeline, and recent changes.
2. Check the highest-signal evidence: status, events, logs, metrics, traces, and configuration.
3. Isolate the layer: client, DNS, network, platform, application, dependency, or data.
4. Apply the least risky mitigation and communicate status.
5. Fix the root cause, validate recovery, and add prevention.
