# EvaNote

Web-based reactive computing platform

## Getting Started

```sh
    git clone --recurse-submodules https://github.com/sanchezcarlosjr/EvaNote.git
    npm install
```

### Environment and settings

Through the use of `.gitignore`, you can create a `.env` file to support a backend with supabase, a PostgreSQL database. Go to [supabase](https://supabase.com/) to create (a) create an organization, (b) create a project, and (c) get the Project URL and API Key. Then, create a copy of the `.env.development.example`, naming it `.env`, `.env.development.local`, or an alternative based on the `.gitignore` file, and update the `VITE_SUPABASE_URL` and `VITE_SUPABASE_ANON_API_KEY` respectively with the Project URL and API Key. This file is loaded at execution time using a template engine and Browser JSON Parser.

#### Run in Supabase SQL Editor

```sql
CREATE TABLE resources (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name TEXT NOT NULL,
    meta JSONB,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE access (
    id SERIAL PRIMARY KEY,
    user_id UUID NOT NULL,
    resource_id UUID NOT NULL,
    access_level TEXT NOT NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT fk_access_resources FOREIGN KEY (resource_id)
    REFERENCES resources(id)
    ON DELETE CASCADE
);

CREATE TABLE resource_permissions (
    id SERIAL PRIMARY KEY,
    user_id UUID NOT NULL,
    resource_id UUID NOT NULL,
    access_level TEXT NOT NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT fk_permissions_resources FOREIGN KEY (resource_id)
    REFERENCES resources(id)
    ON DELETE CASCADE
);

CREATE TABLE valid_parent (
    id SERIAL PRIMARY KEY,
    resource_id UUID NOT NULL,
    parent_id UUID NOT NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT fk_valid_parent_resource FOREIGN KEY (resource_id)
    REFERENCES resources(id)
    ON DELETE CASCADE,
    CONSTRAINT fk_valid_parent_parent FOREIGN KEY (parent_id)
    REFERENCES resources(id)
    ON DELETE CASCADE,
    CONSTRAINT unique_resource_parent UNIQUE (resource_id, parent_id)
);
```

### Available Scripts

```bash
    # Running the development server.
    npm run dev
    # Building for production.
    npm run build
    # Running the production server.
    npm run start
```

## Learn More

To learn more about **Refine**, please check out the [Documentation](https://refine.dev/docs)

- **Supabase Data Provider** [Docs](https://refine.dev/docs/core/providers/data-provider/#overview)
- **Material UI** [Docs](https://refine.dev/docs/ui-frameworks/mui/tutorial/)
- **Inferencer** [Docs](https://refine.dev/docs/packages/documentation/inferencer)
- **i18n** [Docs](https://refine.dev/docs/core/providers/i18n-provider/)
- **React Router** [Docs](https://refine.dev/docs/core/providers/router-provider/)

## License

MIT
