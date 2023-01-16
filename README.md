# fastapi-services-oauth2

This repository provides an approach on how to structure FastAPI application with 
multiple services, simple OAuth2 authentication, and Postgres backend.

## Structure overview

Application consists of 5 packages. Service related functionality is provided by 
`routers`, `services`, `schemas`, and `models` packages. Adding a new service requires 
adding a new module in each of these packages. 

```bash
    .
    └── app/
        ├── backend             # Backend functionality and configs
        |   ├── config.py           # Configuration settings
        │   └── database.py         # Database session manager
        ├── models              # SQLAlchemy models
        │   ├── auth.py             # Authentication models
        |   ├── base.py             # Base classes, mixins
        |   └── ...                 # Service models
        ├── routers             # API routes
        |   ├── auth.py             # Authentication routers
        │   └── ...                 # Service routers
        ├── schemas             # Pydantic models
        |   ├── auth.py              
        │   └── ...
        ├── services            # Business logic
        |   ├── auth.py             # Create user, generate and verify tokens
        |   ├── base.py             # Base classes, mixins
        │   └── ...
        ├── cli.py              # Command-line utilities
        ├── const.py            # Constants
        ├── exc.py              # Exception handlers
        └── main.py             # Application runner
```

Package `backend` provides database session manager and configs. In case, application 
communicates not only with database, but also uses other backends (e.g. other API), 
the clients can be placed in `backend`.

Module `cli` provides command-line functionality related to API services but not required
access through API endpoints. It's focused on the tasks that need to be done manually.
For instance, create a new user and store its hashed data in database, in order the
authentication service could later verify user login requests without accessing unencoded 
password.
