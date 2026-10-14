# Doorstep delivery tracking

A customer watches the rider carrying their parcel approach on a map.
The rider's phone reports where it is as they drive, the app works out
that it has arrived without being told, and a photograph at the door
settles what was left and where.

## What finished looks like

A stranger can open the URL, register, watch the rider carrying their
parcel cross a map without pressing refresh, see the pin keep its story
straight when the phone comes out of a tunnel, find a photograph of the
doorstep waiting with nobody having pressed arrived, and be unable to
watch anybody else's rider or their own a minute after it is delivered.
A rider is handed a round that is theirs alone and a phone that reports
where it is the whole way and still has battery at the end of the
shift.

Finished does not mean running on a laptop. It means somebody who has
never met the person who built it can use it on the internet, on a
phone, on mobile data.

## The road map

Eight sprints. Each one makes a different part of that sentence true,
and the order is not arbitrary: take any sprint out and the sentence
stops being true.

| # | Sprint | What it makes possible |
|---|---|---|
| 1 | Signing on for a shift | Customers and riders are different people, and a phone stays signed in all day |
| 2 | A parcel with a name on it | A parcel belongs to one customer and one rider, and only one rider |
| 3 | A phone that says where it is | Position reported for the whole drive, kept, and paid for |
| 4 | Watching it move | The customer's map moves with nobody pressing anything |
| 5 | Four minutes in a tunnel | A phone that lost signal catches up without the pin going backwards |
| 6 | At the door | Arrival decided from where the rider is, with a photograph to prove it |
| 7 | Who may watch, and for how long | Only your own rider, only while the parcel is out |
| 8 | Out on real roads | It is on the internet, and the live map survives being there |

## The hard part

Three of them, and they are the reason this product is worth building
rather than reading about.

A rider's phone sends a position every few seconds for hours, so this
writes more than anything else you have built, and keeping every one of
them forever is a decision with a bill attached.

A phone that spends four minutes in a tunnel sends everything at once
and out of order when it comes out, and the customer's pin must not
move backwards while it does.

Six customers watching six riders must each see one rider, theirs, and
only while their parcel is out. A live position is a person, not an
order status, and the boundary closing is something you should be able
to demonstrate.

## Working on it

The sprints, their briefs and the tickets under them are in Blacksmith.
Start with sprint one; each sprint opens as the one before it closes.

The reading attached to a sprint is worth opening before its first
ticket rather than after. It carries the parts a ticket deliberately
does not: what the alternatives were, and which one you are choosing
between.

## What this is built with

The whole product: an API and the screens that use it.

- **Express 5** in **TypeScript** for the API.
- **Prisma** for the models and the migrations, against the schema in `prisma/schema.prisma`.
- **Zod** for validating every request, and **zod-to-openapi** for the OpenAPI schema, served at `/api/schema/` and browsable at `/api/docs/`.
- **JWT** for authentication, in `src/modules/auth`.
- **React** with **Vite** for the dev server and the build.
- **Chakra UI** for components, and **React Router** for routes.
- **TanStack Query** for every call to the API, so caching and refetching are decided in one place.

## Setting it up

You need the CLI once: `npm install -g blacksmith-cli`.

```bash
blacksmith setup     # dependencies, Prisma client, migrations
blacksmith dev       # start it
```

The API answers on `http://localhost:8000`, and the app on `http://localhost:5173`.

Copy `backend/.env.example` to `backend/.env` before the first run. It is ignored by git and holds the JWT secret, the database URL and anything else this project should not carry in its history.

## Where the code lives

```
backend/
├── prisma/
│   └── schema.prisma    # your models, and the migrations they generate
├── src/
│   ├── config/          # environment, OpenAPI, Zod setup
│   ├── db/              # the Prisma client, made once
│   ├── middleware/      # authentication, validation, error handling
│   ├── modules/
│   │   └── auth/        # register, log in, refresh
│   ├── utils/           # errors, pagination, tokens
│   ├── app.ts           # where routes are mounted
│   └── index.ts
├── package.json
└── tsconfig.json

frontend/
└── src/
    ├── api/
    │   ├── generated/   # written by `blacksmith sync` — do not edit
    │   └── hooks/       # your queries and mutations
    ├── pages/           # one folder per page
    ├── features/        # auth, and anything else that spans pages
    ├── router/          # routes and layouts
    ├── shared/          # components and hooks used across pages
    └── styles/
```

## Day to day

| Command | What it does |
| --- | --- |
| `blacksmith dev` | Run it locally. |
| `blacksmith sync` | Regenerate the frontend API types and hooks from the backend schema. Run it after changing a Zod schema or a route. |
| `blacksmith make:resource Post` | Scaffold a Prisma model, Zod schemas, a service, a controller and routes, plus the hooks and pages that use them. |
| `blacksmith backend <command>` | Run an npm script in the backend, e.g. `blacksmith backend run migrate`. |
| `blacksmith frontend <command>` | Run an npm command in the frontend, e.g. `blacksmith frontend install axios`. |
| `blacksmith eject` | Remove Blacksmith and keep a plain Express and React project. Nothing here is a dependency on us. |
