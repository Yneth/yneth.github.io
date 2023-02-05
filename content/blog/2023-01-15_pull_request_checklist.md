+++
title = "Pull request checklist"

[taxonomies]
tags = ["code_review"]
+++

For me, it is hard to keep all the rules in the head, so I decided
to compile a list of my checks before publishing a pull-request.

- package version
    - breaking change - use MAJOR bump X.x.x
    - new feature - MINOR x.X.x
    - else -  PATCH x.x.X
- API
    - correct status codes?
    - extendable object?
        <br>
        using objects instead of plain lists or values allows you to extend requests or responses easier.  
        <br>
        *BAD*:  
        ```  
        ["name1", "name"]  
        ```  
        *GOOD*:  
        ```  
        {  
          "data": [{"name": "name1"}]  
        }  
        ```  
        ---  
    - versioned API?
    - swagger / validation annotations
    - openness - who should access the endpoint?
        - public
        - private
    - how to update endpoint?
        - new field
        - new API version
- Configuration
    - feature needs configurability?
    - do not forget to update configurations in the integration tests
- Datasource
    - transactionally correct?
        - what if some system goes down?
        - what if some system rolls back?
        - what if multiple users are editing at the same time?
    - validations are included?
    - all queries are optimal?
    - needs de-normalisation?
    - CAP
        - available
        - consistent
- Messaging
    - Kafka
        - consumer
            - at least once delivery is ok or has correct handling
            - should be consistent? increase number of acks
            - should read from the beginning? use default earliest offsets
- Caching
    - requires caching?
        - ttl?
- Error handling
    - is there something you should handle beforehand?
        - specific DB exceptions
        - specific API exceptions
    - do you need to handle exceptions generically?
    - do you need to handle exceptions separately?
- Security
    - make sure not to include sensitive data in logs
    - sensitive data excluded from `toString`
- Monitoring
    - metrics
        - API
        - Datasource
        - Cache
        - HTTP Clients
    - logs
        - method start traced correctly
        - method completion traced correctly
        - errors are logged once with `incidentId`, log or re-throw strategy
        - warnings for unusual behaviours
    - should include admin endpoint to view the state if any?
- Tests
    - all cases covered?
    - contracts
        - should other services update contracts as well?
- Networking
    - retries
        - idempotency handled?
        - is using correct retry mechanism in case of service discovery?
    - timeouts
    - circuit breaker timeout
        - does it have meaningful fallback to return in case of errors?
- Docs
    - how to
    - configuration options
    - migration
    - REST
    - diagrams
- Third Party users
    - does it break someone?
- Process
    - should you split task and move parts to other sprints?  