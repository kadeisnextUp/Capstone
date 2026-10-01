# Campus Companion

A schedule-aware campus companion app for college students, starting with the
University of Cincinnati. Built as a senior capstone project.

The app takes a student's class schedule as its foundation and builds
everything else around it:

- **Routes between classes:** walking routes and travel times for back-to-back
  classes.
- **Gap suggestions:** study spaces, dining, and campus events you can actually
  reach in the time you have between classes.
- **Orientation:** a guided flow that helps new and transfer students learn
  campus.

## Who it's for

- **Primary:** freshmen and transfer students who don't know campus yet.
- **Secondary:** returning students who want to get around faster.

## The core problem

Ranking and routing: given a location, a time window, and a student's
preferences, work out what is feasible and worth recommending.

## Team

| Name | Role |
| --- | --- |
| Kaden Sawyer | TBD |
| Laila Raines | TBD |

**Faculty advisor:** TBD

## Repository structure

```
/mobile     React Native app
/web        Web application and admin portal
/server     Backend API shared by both clients
/docs       Capstone deliverables (user stories, design docs, reports)
```

## Tech stack

| Layer | Choice |
| --- | --- |
| Mobile | React Native |
| Web | TBD |
| Server | TBD |
| Database | TBD |
| Auth | TBD |
| Maps | Open map data source (TBD) |

## Getting started

The project folders are not scaffolded yet. Setup instructions for each app
will be added here once they are.

## Contributing

- `main` must always be demoable. Don't commit to it directly.
- Branch off `main` with a prefix: `feat/`, `fix/`, or `chore/`. Keep each
  branch to one concern and merge it within a few days.
- Every change goes through a pull request and needs one approval.
- CI runs lint and tests for each folder on every PR (`mobile.yml`, `web.yml`,
  `server.yml` in `.github/workflows/`).

## Privacy

Class schedules are student record data and likely fall under FERPA. The app:

- asks for explicit consent before storing a schedule,
- stores only the minimum data it needs,
- never shares a student's schedule or location with another user unless that
  student opts in, and
- fully deletes data when a student deletes it.

## Documentation

Capstone deliverables live in [`/docs`](docs/):

- [Constraints Essay](docs/Constraints_Essay.md)
- [Professional Biography](docs/Professional_Biography.md)

## Status

- [x] Project idea approved
- [x] Repository set up with CI and branch protection
- [ ] Faculty advisor confirmed
- [ ] User stories and use cases
- [ ] App folders scaffolded
