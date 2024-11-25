# Kubernetes Pod Management Command Reference Guide

- [Section 1: Primary Operations](#section-1-primary-operations)
- [Section 2: Configuration Management](#section-2-configuration-management)
- [Section 3: Service Operations](#section-3-service-operations)
- [Section 4: Monitoring and Logging](#section-4-monitoring-and-logging)
- [Section 5: Maintenance](#section-5-maintenance)
- [Section 6: Security](#section-6-security)
- [Section 7: Optimization](#section-7-optimization)

> **Author**: Md Toriqul Islam  
> **Description**: Comprehensive guide for Kubernetes pod management operations  
> **Learning Focus**: Pod lifecycle management, configuration handling, and troubleshooting  
> **Note**: Test commands in a non-production environment first.

## Section 1: Primary Operations

### Pod Creation
```bash
# Create a basic nginx pod
kubectl run nginx-pod --image=nginx:1.26 --port=80

# Create pod with resource limits
kubectl run nginx-pod --image=nginx:1.26 \
    --requests='cpu=100m,memory=128Mi' \
    --limits='cpu=200m,memory=256Mi'

# Create pod from YAML
kubectl create -f pod-definition.yaml
```

### Pod Management
```bash
# List all pods
kubectl get pods

# Get detailed pod information
kubectl describe pod nginx-pod

# Delete pod
kubectl delete pod nginx-pod

# Force delete pod
kubectl delete pod nginx-pod --force --grace-period=0
```

## Section 2: Configuration Management

### Basic Configuration
```bash
# Export pod configuration
kubectl get pod nginx-pod -o yaml > pod-config.yaml

# Edit pod configuration
kubectl edit pod nginx-pod

# Apply configuration changes
kubectl apply -f pod-config.yaml
```

### Labels and Annotations
```bash
# Add label to pod
kubectl label pod nginx-pod env=production

# Add annotation
kubectl annotate pod nginx-pod description="Production nginx pod"

# Remove label
kubectl label pod nginx-pod env-
```

## Section 3: Service Operations

### Port Management
```bash
# Port forward to pod
kubectl port-forward nginx-pod 8080:80

# Expose pod as service
kubectl expose pod nginx-pod --port=80 --type=NodePort
```

### Container Operations
```bash
# Execute command in pod
kubectl exec -it nginx-pod -- /bin/bash

# Copy files to/from pod
kubectl cp nginx-pod:/etc/nginx/nginx.conf ./nginx.conf
```

## Section 4: Monitoring and Logging

### Pod Monitoring
```bash
# Watch pod status
kubectl get pods -w

# Get pod metrics
kubectl top pod nginx-pod

# Get pod events
kubectl get events | grep nginx-pod
```

### Log Management
```bash
# View pod logs
kubectl logs nginx-pod

# Stream pod logs
kubectl logs -f nginx-pod

# Get previous container logs
kubectl logs nginx-pod --previous
```

## Section 5: Maintenance

### Health Checks
```bash
# Describe pod health status
kubectl describe pod nginx-pod | grep -A 5 Conditions

# Check readiness/liveness probes
kubectl get pod nginx-pod -o yaml | grep -A 10 readinessProbe
```

### Cleanup Operations
```bash
# Delete all pods in namespace
kubectl delete pods --all

# Delete pods by label
kubectl delete pods -l app=nginx
```

## Section 6: Security

### Access Control
```bash
# Get pod security context
kubectl get pod nginx-pod -o yaml | grep -A 10 securityContext

# Create pod with security context
kubectl run secure-pod --image=nginx \
    --security-context.runAsUser=1000 \
    --security-context.runAsNonRoot=true
```

### Resource Restrictions
```bash
# Set resource quotas
kubectl create quota pod-quota \
    --hard=cpu=1,memory=1G,pods=2

# View resource usage
kubectl describe resourcequota pod-quota
```

## Section 7: Optimization

### Performance Tuning
```bash
# Update resource limits
kubectl patch pod nginx-pod -p \
    '{"spec":{"containers":[{"name":"nginx","resources":{"limits":{"cpu":"300m","memory":"512Mi"}}}]}}'

# Set pod priority
kubectl patch pod nginx-pod -p '{"spec":{"priority":100}}'
```

## Learning Notes

1. Always verify pod status after operations
2. Use resource limits to prevent resource exhaustion
3. Implement proper health checks for reliability
4. Maintain security contexts for pods
5. Regular monitoring of pod metrics

---

> 💡 **Best Practice**: Always use declarative YAML files for pod management

> ⚠️ **Warning**: Force deletion may cause data loss or corruption

> 📝 **Note**: Keep pod configurations version controlled

## Troubleshooting Guide

### Common Issues
```bash
# Check pod status
kubectl get pod nginx-pod

# View pod events
kubectl describe pod nginx-pod

# Check container logs
kubectl logs nginx-pod
```

### Error Resolution
```bash
# Restart pod
kubectl replace --force -f pod-definition.yaml

# Debug with temporary pod
kubectl run debug-pod --rm -it --image=busybox -- /bin/sh
```
