# SVI 측정툴 배포 및 DB 설정

## 배포 파일

- `index.html`
- `config.js`

두 파일을 같은 폴더에 올려야 합니다.

## Supabase DB 만들기

Supabase에서 새 프로젝트를 만든 뒤, SQL Editor에서 아래 SQL을 실행하세요.

```sql
create table if not exists public.svi_companies (
  id text primary key,
  name text not null default '새 기업',
  ceo text,
  org_type text,
  address text,
  region text,
  cert_no text,
  social_purpose text,
  total numeric default 0,
  grade text,
  missing integer default 14,
  updated_at timestamptz default now(),
  data jsonb not null default '{}'::jsonb
);

alter table public.svi_companies enable row level security;

create policy "public read svi companies"
on public.svi_companies
for select
using (true);

create policy "public insert svi companies"
on public.svi_companies
for insert
with check (true);

create policy "public update svi companies"
on public.svi_companies
for update
using (true)
with check (true);

create policy "public delete svi companies"
on public.svi_companies
for delete
using (true);
```

## Supabase 연결값 넣기

Supabase 프로젝트의 `Project Settings > API`에서 아래 값을 확인합니다.

- Project URL
- anon public key

그 다음 `config.js`를 열어 아래처럼 입력합니다.

```js
window.SVI_SUPABASE_URL = "https://프로젝트ID.supabase.co";
window.SVI_SUPABASE_ANON_KEY = "anon-public-key";
window.SVI_ADMIN_PASSWORD = "1127";
```

주의: `service_role` 키는 절대 넣지 마세요.

## 관리자 비밀번호

사이트 첫 화면에서 관리자 비밀번호를 1회 입력해야 기업 목록을 볼 수 있습니다.

현재 기본값은 `1127`이며, `config.js`의 `SVI_ADMIN_PASSWORD` 값을 바꾸면 됩니다.

주의: 이 비밀번호는 화면 진입을 막는 간단한 프론트 잠금입니다. 주소를 아는 사람의 DB 접근을 완전히 차단하려면 Supabase Auth와 사용자별 Row Level Security 정책을 추가해야 합니다.

## Vercel 배포

1. GitHub 저장소에 `index.html`, `config.js`를 올립니다.
2. Vercel에서 해당 저장소를 연결합니다.
3. Framework Preset은 `Other` 또는 정적 사이트로 둡니다.
4. Build Command는 비워둡니다.
5. Output Directory는 `/` 또는 비워둡니다.
6. 배포합니다.

## 저장 방식

- `config.js`에 Supabase 값이 있으면 모든 사용자가 같은 DB를 봅니다.
- Supabase 값이 비어 있으면 기존처럼 각 브라우저의 localStorage에만 저장됩니다.
