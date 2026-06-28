# Vanced Work Subagent Playbook

Use subagents when the user asks for feedback agents, web development agents, design review, release review, or multi-agent collaboration.

## Recommended Roles

### Feedback Agent

Use for:

- UX friction diagnosis.
- Risk-focused review before implementation.
- Checking whether proposed UI changes add unnecessary complexity.

Output:

- current user problem
- primary user goal
- highest-impact fixes
- up to 5 feedback items

### Web Development Agent

Use for:

- scoped implementation
- regression-risk review
- functionality preservation checks

Output:

- files changed or recommended
- verification performed
- remaining risks

### Designer Agent

Use for:

- visual system and tone checks
- consistency with the current glass/light SaaS interface
- component styling feedback

Project limit:

- do not rewrite full HTML
- prefer CSS-only changes unless behavior or markup is required

### Release Manager Agent

Use for:

- GitHub sync and deployment checks
- Firebase/GitHub operational risk review
- final links and folder path verification

## Two-Round Collaboration Pattern

1. Feedback agent diagnoses issues.
2. Web development agent proposes or applies scoped fixes.
3. Feedback agent reviews the revised result.
4. Web development agent finalizes and verifies.
5. Release manager checks deploy readiness when GitHub or Firebase is involved.

Keep each round short and tied to the concrete user request.
