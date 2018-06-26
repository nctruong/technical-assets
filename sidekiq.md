---
layout: page
title: Sidekiq
---

## Checking Jobs if existing?

Rspec for sidekiq: we shouldn't use sidekiq in spec, we run body of perform function directly instead.
```ruby
def self.existed?
    job_in_queues = Sidekiq::Queue.new.any? { |job| job.display_class == EvaluationsJob::SynchronizeEvaluationsJob.name }
    job_in_workers = Sidekiq::Workers.new.any? { |process, thread, msg| Sidekiq::Job.new(msg['payload']).display_class == EvaluationsJob::SynchronizeEvaluationsJob.name }
    job_in_queues || job_in_workers
end
```