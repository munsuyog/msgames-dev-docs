# Development Workflow Guide

## Development Workflow Table

| Step | Role | Action | Notes |
|------|------|--------|--------|
| 1 | Developer | **Clone Repo**: Clone the repo from GitHub. | Command: `git clone <repo-url>` |
| 2 | Developer | **Create Feature Branch**: Create a new branch from candidate. | Command: `git checkout -b feature/<project-name>-<page-section>-<description>` |
| 3 | Developer | **Develop Feature**: Work on the feature locally. | Follow folder structure. Unit testing is done here. |
| 4 | Developer | **Sync with Remote**: Get latest changes from candidate branch. | Command: `git pull origin candidate` |
| 5 | Developer | **Push Changes**: Push your changes to remote branch. | Command: `git push origin feature/<branch-name>` |
| 6 | Developer | **Create Pull Request**: Open PR for review. | Include detailed description and testing notes. |
| 7 | Reviewer | **Code Review**: Review the changes. | Check code quality, tests, and documentation. |
| 8 | Developer | **Address Reviews**: Make requested changes. | Update based on feedback. |
| 9 | Maintainer | **Merge**: Merge approved changes into candidate. | After all checks pass and approvals received. |

## Detailed Workflow Steps

### 1. Clone Repository
```bash
# Clone the repository
git clone <repo-url>
cd msgames
```

### 2. Create Feature Branch
```bash
# Get latest candidate branch
git checkout candidate
git pull origin candidate

# Create and switch to new feature branch
git checkout -b feature/beergame-dashboard-analytics
```

Branch naming convention:
- `feature/<project>-<page-section>-<description>` - For new features (e.g., `feature/beergame-auth-login`)
- `bugfix/<project>-<issue-id>-<description>` - For bug fixes (e.g., `bugfix/beergame-1234-header`)
- `docs/<project>-<description>` - For documentation changes
- `refactor/<project>-<description>` - For code refactoring

### 3. Development Phase
- Follow the project structure
- Write unit tests
- Keep commits atomic and meaningful
- Run local tests frequently

### 4. Staying Updated
```bash
# Get latest changes from candidate
git pull origin candidate

# If conflicts occur
git status
# Resolve conflicts
git add .
git commit -m "resolve conflicts with candidate"
```

### 5. Push Changes
```bash
# Push your changes
git push origin feature/beergame-dashboard-analytics
```

### 6. Pull Request Creation
When creating a pull request:
- Use clear title and description
- Reference related issues
- Include testing steps
- Add screenshots if UI changes
- List any dependency changes

### 7. Code Review Process
Reviewers will check for:
- Code quality and standards
- Test coverage
- Documentation updates
- Performance implications
- Security considerations

### 8. Addressing Review Feedback
```bash
# Make requested changes
git add .
git commit -m "address review feedback"
git push origin feature/beergame-dashboard-analytics
```

### 9. Merge Process
Maintainer will:
- Verify all checks pass
- Ensure required approvals
- Check for conflicts
- Merge using squash or rebase into candidate branch
- After testing in candidate, merge to release for production

## Best Practices

### Commits
- Use clear, descriptive commit messages
- Follow conventional commits format
- Keep commits focused and atomic

### Code Quality
- Follow style guides
- Use linters and formatters
- Comment complex logic
- Update documentation
- Include the installation and building documentations

### Branch Management
- Keep branches up to date with candidate
- Delete branches after merge
- Don't reuse branches
- Always branch from candidate

## Common Issues and Solutions

### Merge Conflicts
1. Pull latest candidate
2. Resolve conflicts locally
3. Test after resolution
4. Push changes

### Failed Checks
1. Review error logs
2. Fix locally
3. Run tests
4. Push updates

## Quick References

### Common Commands
```bash
# Check status
git status

# Update branch
git pull origin candidate

# Create branch
git checkout -b feature/beergame-auth-login

# Stage changes
git add .

# Commit
git commit -m "feat(auth): implement login form"

# Push
git push origin feature/beergame-auth-login
```