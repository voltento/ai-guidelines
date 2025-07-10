# AI Code Review Guidelines

## Core Review Principles
- **Performance First**: Every line impacts HFT performance - review with zero-allocation mindset
- **Precision**: Be specific, actionable, and concise in feedback
- **Context Aware**: Understand the HFT domain requirements

## Review Checklist

### 1. Performance Critical Items
- [ ] **Memory Allocations**: Flag any unnecessary allocations (use `go build -gcflags=-m` mentally)
- [ ] **Hot Path Optimization**: Ensure critical trading paths are allocation-free
- [ ] **String Operations**: Prefer `strings.Builder` over concatenation, avoid `fmt.Sprintf` in hot paths
- [ ] **Slice/Map Usage**: Check for proper pre-allocation with `make([]T, 0, capacity)`
- [ ] **Interface Boxing**: Minimize interface{} usage in performance-critical code

### 2. Code Quality
- [ ] **Error Handling**: All errors properly handled, no silent failures
- [ ] **Resource Cleanup**: Proper defer usage for cleanup (connections, files, etc.)
- [ ] **Concurrency Safety**: Race condition analysis, proper mutex usage
- [ ] **Naming**: Clear, domain-specific naming (trading terminology)

### 3. Testing Requirements
- [ ] **Unit Tests**: New code has corresponding tests
- [ ] **Coverage**: Critical services maintain 90%+ coverage
- [ ] **Mocking**: Dependencies properly mocked with gomock
- [ ] **Edge Cases**: Boundary conditions tested

### 4. Architecture Compliance
- [ ] **Domain Boundaries**: Proper separation between domain/infrastructure layers
- [ ] **Dependency Direction**: Dependencies point inward (Clean Architecture)
- [ ] **Interface Segregation**: Interfaces are focused and minimal

## Review Response Format

### For Performance Issues:
```
🚨 PERFORMANCE: [Brief description]
Location: [file:line]
Issue: [Specific problem]
Fix: [Concrete solution]
Impact: [Performance implication]
```

### For Code Quality Issues:
```
💡 QUALITY: [Brief description]
Location: [file:line]
Issue: [What's wrong]
Fix: [How to fix]
```

### For Architecture Issues:
```
🏗️ ARCHITECTURE: [Brief description]
Location: [file:line]
Violation: [Which principle violated]
Fix: [Refactoring suggestion]
```

## Review Efficiency Rules
1. **One Issue Per Comment**: Don't bundle multiple issues
2. **Code Examples**: Provide concrete fix examples when possible
3. **Priority Levels**: Use emoji indicators (🚨 Critical, 💡 Important, 📝 Minor)
4. **No Nitpicking**: Focus on performance, correctness, and maintainability

## HFT-Specific Considerations
- **Latency Impact**: Consider microsecond-level performance implications
- **Market Data Processing**: Ensure efficient handling of high-frequency data streams
- **Order Management**: Review for proper order lifecycle management
- **Risk Controls**: Verify risk management logic is bulletproof
- **Connectivity**: Check for proper connection pooling and reuse

## Quick Reference Commands
- Performance analysis: `go build -gcflags=-m`
- Race detection: `go test -race`
- Coverage: `go test -cover`
- Benchmarking: `go test -bench=.`