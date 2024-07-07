### How we can create a commands in django
```
create a commands module on management/commands/name.py
```
```
class Command(BaseCommand):
    help="Simple cron jobs"
    def handle(self, *args, **options):
        self.stdout.write("Hello from custom command!")
```

### Cron Jobs 
1. Create a task/command
2. Install django-crontab module
```
pip install django-crontab
``` 
3. Add to Installed Apps in settings
```
INSTALLED_APPS = [
    ...
    'django_crontab',
]
```
4. Define Cron Jobs in Settings
```
CRONJOBS = [
    ('0 0 * * *', 'your_app.management.commands.my_cron_job.Command'),
]
```
5. Add and Remove Cron Jobs
```
python manage.py crontab add
```
* Note: We can add our jobs directly in crontab file [Not recommended]

### Describe '* * * * *'

* Minute (0-59): Specifies the exact minute when the job will run.
* Hour (0-23): Specifies the hour of the day (using a 24-hour clock) when the job will run.
* Day of Month (1-31): Specifies the day of the month when the job will run.
* Month (1-12): Specifies the month when the job will run.
* Day of Week (0-6): Specifies the day of the week when the job will run (0 for Sunday, 1 for Monday, ..., 6 for Saturday).

#### Examples
1. 0 0 * * * - Every day at midnight.
2. 30 14 * * * - Every day at 2:30 PM.
3. 0 22 * * 1-5 - Every weekday (Monday to Friday) at 10:00 PM.
4. 0 8 1 * * - At 8:00 AM on the first day of every month.
5. */15 * * * * - Every 15 minutes.

