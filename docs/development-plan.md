# FIX Protocol Monitor - Development Notes

## Coding Practices

### Documentation
- Include clear docstrings for all functions and classes
- Use Google-style docstring format for consistency
- Add inline comments for complex logic only (avoid over-commenting obvious code)
- Document any FIX protocol specifics that may not be common knowledge

### Code Quality Guidelines
- Use type hints to improve code clarity and catch potential issues early
- Write concise, readable code using Python's built-in features
- Follow standard Python conventions (PEP 8)
- Keep functions focused on single responsibilities for easier testing and maintenance
- Use domain-specific terminology in variable names to make code more self-documenting

```python
# Example showing practical implementation approach
def extract_reject_messages(log_content: str) -> list[dict]:
    """
    Extract reject messages from FIX log content.
    
    Args:
        log_content: Raw FIX log content as string
        
    Returns:
        List of extracted reject messages with parsed fields
    """
    # Extract messages with tags we're monitoring
    patterns = {
        "SessionReject": r"35=3\x01",
        "OrderReject": r"35=8\x01.*39=8\x01",
        "BusinessReject": r"35=j\x01",
        "DontKnowTrade": r"35=q\x01"
    }
    
    results = []
    for line in log_content.splitlines():
        for msg_type, pattern in patterns.items():
            if re.search(pattern, line):
                results.append({
                    "type": msg_type,
                    "content": line,
                    "timestamp": datetime.now()
                })
                break
                
    return results
```

### Security Considerations
- This application is designed for UAT environments only
- Note: No authentication is implemented as this is intended for internal, controlled environments
- FIX messages may contain sensitive information - ensure the application is run in a secure environment
- Do not log full FIX messages to external files without data sanitization
- Consider implementing role-based access if deploying beyond UAT

### Performance Optimizations
- Use Watchdog's native event handlers rather than polling when possible
- Implement efficient message parsing using regular expressions compiled once at startup
- For future enhancements, consider background processing for analytics to maintain UI responsiveness
- Buffer read operations to minimize file I/O impact

### Testing Strategy
- Unit tests for message parsing logic
- Integration tests for file monitoring components
- Mock file changes to test real-time detection capabilities

## Future Enhancement Notes

### Data Storage Considerations
- Use SQLite for the initial database implementation (lightweight, no separate server needed)
- Design schema to allow for efficient time-based queries
- Include proper indexing for message types and timestamps
- Future option: Consider migration path to PostgreSQL if volume becomes significant

### Grafana Integration
- Export metrics via Prometheus-compatible format
- Define standard dashboards for common monitoring scenarios
- Consider using InfluxDB for time-series data when implementing historical analytics

### Compliance Considerations
- Add configurable data retention policies
- Implement data sanitization for any externally shared logs
- Document usage restrictions (UAT only) prominently in README and application startup

## Development Approach
- Build core monitoring functionality first
- Add persistence layer second
- Implement visualization and analytics as third phase
- Consider containerization for easier deployment
