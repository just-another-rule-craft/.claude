# Phoenix Security Audit

Comprehensive security audit and vulnerability assessment for Phoenix applications.

**Usage**: `/project:security-audit [scope or specific security concern]`

**Arguments**:
- `$ARGUMENTS`: Specific security area to audit or general security review

## Workflow

Perform security audit for: $ARGUMENTS

Follow this systematic security assessment:

### Phase 1: Security Context Assessment
Use "think hard" to understand the security landscape:
- What sensitive data does the application handle?
- What are the attack vectors and threat models?
- What compliance requirements apply?
- What external integrations exist?

### Phase 2: Authentication & Authorization Audit
**Identity Management**:
```
Use `get_source_location` to examine:
- User authentication mechanisms
- Password handling and storage
- Session management
- Multi-factor authentication implementation
- OAuth/OIDC integration security

Use `get_ecto_schemas` to verify:
- User schema security fields
- Password hashing implementation
- Account lockout mechanisms
- Session token management
```

**Authorization Patterns**:
```
Use `get_source_location` to audit:
- Role-based access control (RBAC)
- Permission checking patterns
- Resource-level authorization
- API endpoint protection
- Admin interface security
```

### Phase 3: Input Validation & Data Protection
**Input Sanitization**:
```
Use `get_source_location` to check:
- Form parameter validation
- JSON API input validation
- File upload restrictions
- Query parameter sanitization
- Header validation
```

**SQL Injection Prevention**:
```
Use `execute_sql_query` and `get_source_location` to verify:
- Parameterized queries usage
- Ecto changeset validations
- Raw SQL query safety
- Dynamic query construction
```

**XSS Prevention**:
```
Check template rendering for:
- Proper output escaping
- Safe HTML rendering
- User-generated content handling
- JavaScript injection vectors
- Template injection vulnerabilities
```

### Phase 4: CSRF & Request Forgery Protection
**CSRF Analysis**:
```
Use `get_source_location` to verify:
- CSRF token implementation in forms
- API endpoint CSRF protection
- State-changing operation protection
- Token validation mechanisms
```

**Same-Origin Policy**:
```
Audit CORS configuration:
- Allowed origins validation
- Credential handling in CORS
- Preflight request handling
- WebSocket origin validation
```

### Phase 5: Session & Cookie Security
**Session Management**:
```
Use `project_eval` to test:
- Session token randomness
- Session expiration handling
- Secure session storage
- Session fixation prevention
- Concurrent session limits
```

**Cookie Security**:
```
Verify cookie attributes:
- HttpOnly flag implementation
- Secure flag for HTTPS
- SameSite attribute configuration
- Domain and path restrictions
```

### Phase 6: Data Exposure & Privacy
**Sensitive Data Handling**:
```
Use `get_ecto_schemas` and `get_source_location` to audit:
- Personal data encryption
- Credit card information handling
- API key and secret storage
- Database encryption at rest
- Log sanitization
```

**Information Disclosure**:
```
Check for information leakage in:
- Error messages and stack traces
- Debug information exposure
- API response verbosity
- URL parameter exposure
- HTTP header information
```

### Phase 7: Infrastructure Security
**Configuration Security**:
```
Use `get_source_location` to review:
- Environment variable usage
- Secret management practices
- Database connection security
- External service authentication
- SSL/TLS configuration
```

**Dependency Security**:
```
Audit third-party packages:
- Known vulnerability scanning
- Dependency version management
- Package integrity verification
- License compliance
```

### Phase 8: API Security
**API Endpoint Security**:
```
Use `get_source_location` to examine:
- Rate limiting implementation
- API authentication methods
- Request/response validation
- Error handling in APIs
- Versioning security considerations
```

**GraphQL Security** (if applicable):
```
Check GraphQL implementation for:
- Query depth limiting
- Query complexity analysis
- Introspection disabling in production
- Authorization in resolvers
```

### Phase 9: Real-time Security
**Phoenix Channel Security**:
```
Use `get_source_location` to audit:
- Channel authentication
- Topic-level authorization
- Message validation
- Rate limiting for channels
- Connection state management
```

**LiveView Security**:
```
Examine LiveView implementations for:
- State manipulation protection
- Event handler authorization
- CSRF token handling
- Session hijacking prevention
```

### Phase 10: Monitoring & Incident Response
**Security Monitoring**:
```
Use `get_logs` to verify:
- Security event logging
- Failed authentication tracking
- Suspicious activity detection
- Audit trail completeness
```

**Incident Response**:
```
Verify security incident procedures:
- Alert mechanisms
- Response playbooks
- Data breach procedures
- Recovery processes
```

## Security Testing Techniques

### Penetration Testing
```
Use `project_eval` to simulate attacks:
- SQL injection attempts
- XSS payload testing
- CSRF attack simulation
- Authentication bypass attempts
- Authorization escalation tests
```

### Vulnerability Scanning
```bash
# Run security scanners
mix deps.audit              # Dependency vulnerability scan
mix sobelow                 # Phoenix security scanner
credo --strict              # Code quality and security
```

### Security Headers Testing
```
Verify security headers:
- Content-Security-Policy
- X-Frame-Options
- X-XSS-Protection
- Strict-Transport-Security
- X-Content-Type-Options
```

## Common Security Vulnerabilities

### OWASP Top 10 for Phoenix
1. **Injection**: SQL, NoSQL, OS command injection
2. **Broken Authentication**: Session management flaws
3. **Sensitive Data Exposure**: Inadequate encryption
4. **XML External Entities**: XML parsing vulnerabilities
5. **Broken Access Control**: Authorization bypass
6. **Security Misconfiguration**: Default/weak configurations
7. **Cross-Site Scripting**: XSS in templates
8. **Insecure Deserialization**: Unsafe object deserialization
9. **Known Vulnerabilities**: Outdated dependencies
10. **Insufficient Logging**: Inadequate security monitoring

### Phoenix-Specific Security Issues
- **LiveView State Manipulation**: Client-side state tampering
- **Channel Authorization**: Inadequate topic-level security
- **Plug Pipeline Security**: Missing security plugs
- **Ecto Query Injection**: Dynamic query construction flaws

## Security Checklist

**Authentication & Sessions**:
- [ ] Passwords properly hashed (bcrypt/argon2)
- [ ] Session tokens cryptographically random
- [ ] Account lockout after failed attempts
- [ ] Secure session storage and expiration
- [ ] Multi-factor authentication where appropriate

**Authorization**:
- [ ] All endpoints properly authorized
- [ ] Role-based access control implemented
- [ ] Resource-level permissions enforced
- [ ] Admin functions restricted and audited

**Input Validation**:
- [ ] All user inputs validated and sanitized
- [ ] File uploads restricted and scanned
- [ ] SQL injection prevention verified
- [ ] XSS protection implemented

**Data Protection**:
- [ ] Sensitive data encrypted at rest
- [ ] Secure communication (HTTPS/TLS)
- [ ] Personal data handling compliant
- [ ] Audit trails for sensitive operations

**Infrastructure**:
- [ ] Dependencies regularly updated
- [ ] Security headers implemented
- [ ] Error handling doesn't leak information
- [ ] Production configurations hardened

**Monitoring**:
- [ ] Security events logged and monitored
- [ ] Incident response procedures documented
- [ ] Regular security assessments scheduled
- [ ] Vulnerability management process established

## Compliance Considerations

### GDPR/Privacy
- Data minimization principles
- Consent management
- Data retention policies
- Right to deletion implementation

### PCI DSS (if handling payments)
- Secure cardholder data storage
- Strong access controls
- Network security measures
- Regular security testing

### HIPAA (if handling health data)
- PHI encryption requirements
- Access logging and auditing
- Business associate agreements
- Risk assessments

Remember: Security is not a one-time task but an ongoing process. Regular audits, updates, and monitoring are essential for maintaining a secure Phoenix application.
