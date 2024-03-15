# Loan Application PDF Generator

A Rails workflow for collecting a loan request through a multi-step form, generating a PDF term sheet, and emailing the result from a background job.

## Workflow

```text
multi-step loan form -> persisted LoanRequest -> LoanProcessorJob
                                           -> PDF term sheet
                                           -> UserMailer delivery
```

The Stimulus form controller manages client-side steps, Rails validates and stores the request, Wicked PDF renders `termsheet.pdf.erb`, and the job/mailer boundary handles delivery.

## Local setup

Requirements include Ruby 3.1.3, PostgreSQL, Redis, Node.js, and the native PDF renderer expected by Wicked PDF.

```bash
bundle install
bundle exec rails db:create db:migrate
bundle exec sidekiq
./bin/dev
```

Configure database, mail delivery, Redis, and application secrets for the local environment before submitting a request.

## Project map

- `app/controllers/loan_requests_controller.rb` — request workflow.
- `app/javascript/controllers/loan_request_form_controller.js` — multi-step form behavior.
- `app/jobs/loan_processor_job.rb` — asynchronous processing.
- `app/views/loan_requests/termsheet.pdf.erb` — PDF template.
- `app/mailers/user_mailer.rb` — report delivery.

## Boundaries

- Generated documents are illustrative and not financial advice or a binding lending decision.
- Email/PDF success depends on correctly configured external processes and native binaries.
- Background-job retries and idempotency should be verified before production use.
