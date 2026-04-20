# Policy: Environment Isolation

**Category:** Supporting policy block
**Purpose:** Shrink the blast radius when the agent gets something wrong or is successfully injected

## Why it matters

Computer use and code-execution agents reach real systems. A compromised or confused agent with access to the user's personal browser profile, shell environment, or production systems can cause damage that is hard to reverse: cookies exfiltrated, credentials typed into phishing pages, scripts run against live databases.

The defense is containment. The agent should run in an environment where a worst-case action still cannot reach valuable assets. Everything in this block assumes isolation is the default, not a nice-to-have.

## Drop-in block

```markdown
## Environment and scope

### Isolation requirements
- Run the agent in an isolated browser context, container, or VM. Do not share profile, cookies, or extensions with the user's personal environment.
- Pass an empty environment variable set to the browser or runtime so host secrets are not inherited.
- Disable browser extensions and local file-system access where possible.
- Prefer viewport dimensions the model is trained for: 1440x900 or 1600x900 for desktop; 1280x720 for browser-only flows.

### Allow and deny lists
- Maintain an explicit allow list of domains, apps, and actions the agent may use.
- Block everything not on the allow list by default.
- Log every navigation, tool call, and action attempt for audit.

### Human in the loop
- Keep a human reviewer in the loop for: purchases, authenticated flows, destructive actions, anything hard to reverse, and anything touching production systems.
- Treat computer-use reach as a security boundary, not a convenience. The agent can reach any site, form, or workflow a person can.
```

## Practical setup checklist

**Browser agents (Playwright, Selenium, Puppeteer)**
- Launch Chromium with `chromiumSandbox: true`, an empty `env: {}`, and `--disable-extensions --disable-file-system`
- Use a throw-away browser context per session; never reuse user data directories
- Block download prompts by default; route any download through an explicit review step
- Disable geolocation, notifications, and clipboard permissions unless the task requires them

**VM or container agents**
- Use a non-root user with no passwordless sudo for the agent's shell
- Mount only the folders the agent needs; mount read-only wherever possible
- Disable outbound network by default, then add the allow-list domains explicitly
- Snapshot the VM before each session and roll back at the end

**Code-execution harnesses**
- Use a sandboxed execution context (for example Node `vm`, isolated Python subprocess, Firecracker microVM)
- Expose only the helpers the model needs: `browser`, `context`, `page`, a text-output function, an image-output function, and an ask-user tool
- Do not expose raw shell, arbitrary `subprocess`, or full filesystem access unless the task demands it and the policy allows it

## Allow-list design

Good allow lists are:

- **Narrow.** List specific domains, not "anything on the open web."
- **Versioned.** Track changes in source control; treat the allow list as policy code.
- **Enforced.** The runtime, not just the prompt, should block disallowed navigation.
- **Logged.** Every allow-list hit or miss is an audit event.

Deny-by-default. Allow-by-exception. The prompt reinforces the runtime policy; it does not substitute for it.

## Adaptation notes

- For agents that need to visit user-supplied URLs, add a one-click approval flow that adds the domain to the session's allow list temporarily.
- For enterprise deployments, federate the allow list with your existing web-proxy or SSE domain controls rather than maintaining two sources of truth.
- For regulated workloads (healthcare, finance), add a data-loss-prevention (DLP) check between the agent's action queue and the actual execution.

## Related

- [Pattern 1: Trust boundary](../patterns/01-trust-boundary.md) reduces the likelihood of injection
- [Policy: Turn discipline](./turn-discipline.md) keeps the runtime clean between turns
