# Ecosystem Monitoring Guide

How to set up and use pulse for monitoring ecosystem health.

## Overview

Pulse provides centralized monitoring for all ecosystem repositories:

- Build status tracking
- Dependency vulnerability alerts
- Quality metrics aggregation
- Health score dashboards

## Quick Start

### Install Pulse

```bash
pip install pulse-monitor
```

### Basic Usage

```python
from pulse import EcosystemMonitor

monitor = EcosystemMonitor(org="sebastienrousseau")
health = monitor.check_repo("shokunin")
print(health.score)  # 0-100 health score
```

## Monitoring Features

### Repository Health Score

Each repository gets a health score (0-100) based on:

| Factor | Weight | Criteria |
|--------|--------|----------|
| Build Status | 25% | Last 10 builds success rate |
| Test Coverage | 20% | Coverage percentage |
| Dependencies | 20% | Up-to-date, no vulnerabilities |
| Documentation | 15% | README, API docs present |
| Activity | 10% | Recent commits, issues |
| Security | 10% | No critical vulnerabilities |

### Dependency Tracking

Pulse monitors dependencies across all projects:

```python
# Check for vulnerabilities
vulns = monitor.scan_vulnerabilities()
for vuln in vulns:
    print(f"{vuln.package}: {vuln.severity} - {vuln.description}")
```

### Build Status Aggregation

View build status across the ecosystem:

```python
# Get build status for all repos
statuses = monitor.get_build_statuses()
for repo, status in statuses.items():
    print(f"{repo}: {status.state} ({status.last_run})")
```

## Integration

### GitHub Actions Integration

Add pulse reporting to your CI:

```yaml
# .github/workflows/ci.yml
jobs:
  build:
    # ... your build steps ...

  report:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Report to Pulse
        uses: sebastienrousseau/pulse-action@v1
        with:
          token: ${{ secrets.PULSE_TOKEN }}
          status: ${{ job.status }}
```

### Webhook Integration

Configure webhooks for real-time updates:

```python
# pulse/config.py
WEBHOOKS = {
    "slack": "https://hooks.slack.com/...",
    "discord": "https://discord.com/api/webhooks/...",
}

ALERTS = {
    "critical_vulnerability": ["slack", "discord"],
    "build_failure": ["slack"],
}
```

## Dashboard

### Running the Dashboard

```bash
pulse dashboard --port 8080
```

Access at `http://localhost:8080`

### Dashboard Features

- **Overview:** Ecosystem health at a glance
- **Repositories:** Individual repo metrics
- **Dependencies:** Dependency tree and vulnerabilities
- **Trends:** Health score over time
- **Alerts:** Active issues requiring attention

## Alerting

### Configure Alerts

```python
# pulse/alerts.py
ALERT_RULES = [
    {
        "name": "critical_vulnerability",
        "condition": "vulnerability.severity == 'critical'",
        "channels": ["slack", "email"],
        "frequency": "immediate",
    },
    {
        "name": "build_failure",
        "condition": "build.status == 'failure' and build.branch == 'main'",
        "channels": ["slack"],
        "frequency": "immediate",
    },
    {
        "name": "coverage_drop",
        "condition": "coverage.delta < -5",
        "channels": ["slack"],
        "frequency": "daily_digest",
    },
]
```

### Alert Channels

| Channel | Configuration | Use Case |
|---------|---------------|----------|
| Slack | Webhook URL | Team notifications |
| Discord | Webhook URL | Community updates |
| Email | SMTP settings | Critical alerts |
| PagerDuty | Integration key | On-call alerts |

## Metrics API

### Endpoints

```
GET /api/v1/health                    # Ecosystem overview
GET /api/v1/repos                     # All repositories
GET /api/v1/repos/{name}              # Single repository
GET /api/v1/repos/{name}/metrics      # Detailed metrics
GET /api/v1/vulnerabilities           # All vulnerabilities
GET /api/v1/builds                    # Recent builds
```

### Example Response

```json
{
  "name": "shokunin",
  "health_score": 92,
  "metrics": {
    "build_status": "passing",
    "coverage": 87.5,
    "dependencies": {
      "total": 45,
      "outdated": 2,
      "vulnerable": 0
    },
    "last_commit": "2026-02-18T08:00:00Z"
  }
}
```

## Self-Hosted Setup

### Requirements

- Python 3.11+
- PostgreSQL 14+ (for persistence)
- Redis (for caching)

### Docker Deployment

```yaml
# docker-compose.yml
version: '3.8'
services:
  pulse:
    image: sebastienrousseau/pulse:latest
    ports:
      - "8080:8080"
    environment:
      - DATABASE_URL=postgresql://...
      - REDIS_URL=redis://...
      - GITHUB_TOKEN=${GITHUB_TOKEN}

  postgres:
    image: postgres:14

  redis:
    image: redis:7
```

### Configuration

```bash
# Environment variables
export GITHUB_TOKEN=ghp_...
export DATABASE_URL=postgresql://localhost/pulse
export PULSE_SCAN_INTERVAL=3600  # seconds
```

## Troubleshooting

### Common Issues

**Rate Limiting:**
```
Error: GitHub API rate limit exceeded
```
Solution: Use authenticated requests with `GITHUB_TOKEN`

**Missing Metrics:**
```
Warning: No coverage data for repo
```
Solution: Ensure Codecov integration is configured

**Stale Data:**
```
Warning: Data older than 24 hours
```
Solution: Check scan interval and GitHub connectivity

### Debug Mode

```bash
pulse --debug scan --repo shokunin
```

## Best Practices

1. **Set appropriate alert thresholds** - Avoid alert fatigue
2. **Review dashboard weekly** - Catch trends early
3. **Act on vulnerabilities promptly** - Critical within 24h
4. **Monitor coverage trends** - Prevent gradual decline
5. **Keep pulse updated** - New features and fixes
