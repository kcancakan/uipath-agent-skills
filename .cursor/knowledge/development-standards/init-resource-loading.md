# Init Resource Loading Pattern

## Purpose

Static resources that do not change per transaction should be loaded once during Init instead of being repeatedly loaded during Process execution.

Examples:

- Request body templates
- Static txt/json/xml files
- Static API payload templates
- Mapping files
- Constant lookup resources

The preferred pattern is:

- Load once in Init
- Store in Config or framework-level variables
- Reuse during transaction processing

---

## Standard

Avoid repeatedly reading the same static resource inside Process transaction loops.

Preferred behavior:

```text
Init
↓
Read static template/resource once
↓
Store in Config or framework variable
↓
Reuse during Process
```

Avoid:

```text
Process
↓
Read same txt/template file for every transaction
```

## Why This Standard Exists

Repeated resource loading inside Process creates unnecessary operational overhead.

Possible consequences:

- Repeated file I/O
- Unnecessary disk/network access
- API throttling pressure
- Slower transaction throughput
- Increased memory/resource usage
- Unnecessary external dependency calls
- File lock/read contention

In transactional automations, even lightweight operations become expensive when repeated hundreds or thousands of times.

## Common Scenario

A request body template is stored inside:

`requestBody.txt`

The process:

- Reads the txt file
- Replaces parameter placeholders
- Sends API request

Anti-pattern:

For every transaction:

```text
Read requestBody.txt
Replace parameters
Send API request
```

Preferred pattern:

Init:

```text
Read requestBody.txt once
Store template in Config
```

Process:

```text
Read template from Config
Replace parameters
Send API request
```

## Operational Impact

Repeated template/resource loading may:

- Increase execution duration
- Create API/file pressure
- Increase infrastructure dependency sensitivity
- Reduce scalability
- Make support/performance analysis harder

This becomes especially important in:

- Queue-based transactional processes
- High-volume API automations
- Excel/file-heavy workflows
- Cloud-integrated automations

## Review Guidance

When reviewing processes, check:

- Is the same static resource read repeatedly during Process?
- Does the resource actually change per transaction?
- Can the resource be safely loaded once in Init?
- Is Config/framework storage already available?
- Does repeated loading create unnecessary external dependency usage?

Flag as Suggestion if:

- The impact is minimal
- Transaction count is low

Flag as Warning if:

- Repeated loading happens inside transaction loops
- The process is high-volume
- External dependency pressure increases noticeably

Flag as Critical only if:

- Repeated loading can create operational outage risk
- API throttling/resource exhaustion becomes likely
- File/resource contention causes transaction instability

## Exception Cases

Repeated loading may still be acceptable when:

- The resource changes dynamically during execution
- Each transaction requires a different file/template
- Security/compliance rules require fresh reads
- The resource is intentionally transaction-scoped

Do not apply this pattern blindly without understanding the operational behavior of the resource itself.
