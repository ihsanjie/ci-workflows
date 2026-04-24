# ci-workflows

Kumpulan **reusable GitHub Actions workflows** untuk dipakai ulang lintas repositori milik `ihsanjie`. Tujuannya supaya konfigurasi CI/CD konsisten, terpusat, dan mudah dirawat — cukup update di sini, semua repo ikut terupdate.

## Daftar Workflow

| File | Deskripsi | Trigger |
| --- | --- | --- |
| `.github/workflows/nextjs-ci.yml` | CI untuk proyek Next.js: install, lint, type-check, dan build. | `workflow_call` |

## `nextjs-ci.yml`

Reusable workflow untuk project berbasis **Next.js / TypeScript**. Menjalankan langkah-langkah berikut di `ubuntu-latest`:

1. Checkout kode (`actions/checkout@v4`)
2. Setup Node.js dengan cache `npm` (`actions/setup-node@v4`)
3. Install dependencies via `npm ci`
4. Lint dengan `npm run lint` (non-blocking)
5. Type check dengan `npx tsc --noEmit` (non-blocking)
6. Build dengan `npm run build`

### Inputs

| Nama | Tipe | Wajib | Default | Keterangan |
| --- | --- | --- | --- | --- |
| `node-version` | `string` | tidak | `"20"` | Versi Node.js yang dipakai runner. |

### Cara Pakai

Di repo yang ingin memakai workflow ini, buat file `.github/workflows/ci.yml`:

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:

jobs:
  nextjs-ci:
    uses: ihsanjie/ci-workflows/.github/workflows/nextjs-ci.yml@main
    with:
      node-version: "20"
```

Untuk memakai versi Node yang berbeda:

```yaml
jobs:
  nextjs-ci:
    uses: ihsanjie/ci-workflows/.github/workflows/nextjs-ci.yml@main
    with:
      node-version: "18"
```

> Disarankan memakai **tag rilis** (mis. `@v1`) alih-alih `@main` agar workflow di repo pemanggil tidak ikut berubah setiap ada commit baru di sini.

## Prasyarat pada Repo Pemanggil

Agar workflow berjalan mulus, repo yang memanggil sebaiknya memiliki:

- `package.json` dengan script `lint` dan `build`
- `package-lock.json` (karena memakai `npm ci`)
- Konfigurasi TypeScript (`tsconfig.json`) bila ingin type-check bermakna

## Kontribusi

1. Fork / branch baru dari `main`
2. Tambah atau ubah workflow di `.github/workflows/`
3. Buat Pull Request dengan deskripsi singkat perubahan

## Lisensi

Belum ditentukan. Tambahkan file `LICENSE` jika ingin merilis secara eksplisit.
