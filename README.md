# crontab-check

## Usage

    % crontab-check --file /etc/cron.d/hoehoe
    2019-08-23 17:00 NOW
    2019-08-23 17:05 (+5m / +5m) /opt/hoehoe/bin/jobjob.sh nyo pyo
    2019-08-23 17:08 (+3m / +8m) ...
    :
    :
    INFO: total 7 jobs in 60m (-18:00)

    % crontab-check --file ./crontab-with-something-error
    ERROR: NO EOL at line 42.
