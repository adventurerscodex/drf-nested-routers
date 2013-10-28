**This is a work in progress. It "works for me" at www.apiregistro.com.br, 
but I cannot warranty that it fully "works everywhere" yet.**

rest_framework_nested
=====================

This package provides routers and relations to create nested resources in the
Django Rest Framework

Nested resources are needed for full REST URL structure, if one resource lives inside another.

The following example is about Domains and DNS Nameservers. 
There is many domains, and each have many nameservers. The "nameserver" resource does not
exist without a domain, so you need it "nested" inside the domain.

The desired URL signatures are:
```
\domain\ <- Domains list
\domain\{pk}\ <- One domain, from {pk]
\domain\{domain_pk}\nameservers\ <- Nameservers of domain from {domain_pk}
\domain\{domain_pk}\nameservers\{pk} <- Specific nameserver from {pk}, of domain from {domain_pk}
```

How to do it (example):
```python
# urls.py
from rest_framework_nested import routers
from views import DomainViewSet, NameserverViewSet
(...)

router = routers.SimpleRouter()
router.register(r'domains', DomainViewSet)

domains_router = routers.NestedSimpleRouter(router, r'domains', lookup='domain')
domains_router.register(r'nameservers', NameserverViewSet)

urlpatterns = patterns('',
    url(r'^', include(router.urls)),
    url(r'^', include(domains_router.urls)),
)
```
