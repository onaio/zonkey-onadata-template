# zonkey-onadata-template

Zonkey-branded Django templates for OnaData's user-facing auth pages
(login, logout, password reset, signup). Overrides OnaData's defaults
via the `TEMPLATE_OVERRIDE_ROOT_DIR` setting.

## How it plugs in

This repo is a sibling to the `zonkey` repo. Zonkey's `docker-compose.yml`
bind-mounts this directory into the OnaData api + celery containers at
`/opt/overlay/site_template`. The overlay settings module sets
`TEMPLATE_OVERRIDE_ROOT_DIR = "/opt/overlay/site_template"`; OnaData's
`common.py` then:

- Prepends `/opt/overlay/site_template/templates` to Django's template
  search path (so files in `templates/registration/` override the
  OnaData-shipped equivalents at `onadata/libs/templates/registration/`)
- Appends `/opt/overlay/site_template/static` to `STATICFILES_DIRS`
  (so files in `static/` are served at `/static/<filename>`)

## Layout

````markdown
```
.
├── static/
│   ├── onadata-logo.png
│   ├── favicon-32x32.png
│   └── css/
│       └── ona-auth.css
└── templates/
    ├── base.html
    └── registration/
        ├── login.html
        ├── logout.html
        ├── password_reset_{form,done,confirm,complete}.html
        └── registration_{form,complete}.html
```
````

## Visual treatment

"OnaData, by Ona" posture: light gray background (`#f8f9fa`), OnaData
logo header, centered white card, Zonkey-orange (`#ff5d00`) on primary
buttons and inline links only.

## Changing a template locally

No build step. Edit the template, save, and refresh the browser —
Django re-reads templates on every request in development.

See the zonkey repo's `DEVELOPMENT.md` for the full docker-compose
setup.
