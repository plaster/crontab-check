# crontab-check

## 使い方

### 過去30分に開始したジョブと、直後30分までに開始する予定のジョブを一覧する

    % bin/crontab-check --auto --past 30m --in 30m
    2019-08-25 03:10:00 ------ (NOW-30'30") BEGIN
    2019-08-25 03:17:00 ------ (NOW-23'30") root cd / && run-parts --report /etc/cron.hourly
    2019-08-25 03:40:00 ------ (NOW-0'30") - true
    2019-08-25 03:40:30 ------ (NOW)
    2019-08-25 03:42:00 +1'30" (NOW+1'30") - true
    2019-08-25 03:44:00 +2'00" (NOW+3'30") - true
    2019-08-25 03:46:00 +2'00" (NOW+5'30") - true
    2019-08-25 03:48:00 +2'00" (NOW+7'30") - true
    2019-08-25 03:50:00 +2'00" (NOW+9'30") - true
    2019-08-25 03:52:00 +2'00" (NOW+11'30") - true
    2019-08-25 03:54:00 +2'00" (NOW+13'30") - true
    2019-08-25 04:01:00 +7'00" (NOW+20'30") root /bin/ls -l /tmp >/dev/null
    2019-08-25 04:02:00 +1'00" (NOW+21'30") root /bin/ls -l /tmp >/dev/null
    2019-08-25 04:03:00 +1'00" (NOW+22'30") root /bin/ls -l /tmp >/dev/null
    2019-08-25 04:03:00 +0'00" (NOW+22'30") - true
    2019-08-25 04:04:00 +1'00" (NOW+23'30") root /bin/ls -l /tmp >/dev/null
    2019-08-25 04:04:00 +0'00" (NOW+23'30") - true
    2019-08-25 04:05:00 +1'00" (NOW+24'30") root /bin/ls -l /tmp >/dev/null
    2019-08-25 04:05:00 +0'00" (NOW+24'30") - true
    2019-08-25 04:06:00 +1'00" (NOW+25'30") - true
    2019-08-25 04:07:00 +1'00" (NOW+26'30") - true
    2019-08-25 04:08:00 +1'00" (NOW+27'30") - true
    2019-08-25 04:09:00 +1'00" (NOW+28'30") - true
    2019-08-25 04:10:00 +1'00" (NOW+29'30") root /bin/ls -l /tmp >/dev/null
    2019-08-25 04:10:00 +0'00" (NOW+29'30") END

### コマンドではなく crontab の定義箇所を出力する

    % bin/crontab-check --auto --past 30m --in 30m --print-location
    2019-08-25 03:10:00 ------ (NOW-30'39") BEGIN
    2019-08-25 03:17:00 ------ (NOW-23'39") /etc/crontab:11:17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
    2019-08-25 03:40:00 ------ (NOW-0'39") <USER-CRONTAB>:23:3,4-9,40-55/2 3-12 * * * true
    2019-08-25 03:40:39 ------ (NOW)
    2019-08-25 03:42:00 +1'21" (NOW+1'21") <USER-CRONTAB>:23:3,4-9,40-55/2 3-12 * * * true
    2019-08-25 03:44:00 +2'00" (NOW+3'21") <USER-CRONTAB>:23:3,4-9,40-55/2 3-12 * * * true
    2019-08-25 03:46:00 +2'00" (NOW+5'21") <USER-CRONTAB>:23:3,4-9,40-55/2 3-12 * * * true
    2019-08-25 03:48:00 +2'00" (NOW+7'21") <USER-CRONTAB>:23:3,4-9,40-55/2 3-12 * * * true
    2019-08-25 03:50:00 +2'00" (NOW+9'21") <USER-CRONTAB>:23:3,4-9,40-55/2 3-12 * * * true
    2019-08-25 03:52:00 +2'00" (NOW+11'21") <USER-CRONTAB>:23:3,4-9,40-55/2 3-12 * * * true
    2019-08-25 03:54:00 +2'00" (NOW+13'21") <USER-CRONTAB>:23:3,4-9,40-55/2 3-12 * * * true
    2019-08-25 04:01:00 +7'00" (NOW+20'21") /etc/cron.d/test:1:1-5,10-20/2,30-50/10 0-23/2 * * * root /bin/ls -l /tmp >/dev/null
    2019-08-25 04:02:00 +1'00" (NOW+21'21") /etc/cron.d/test:1:1-5,10-20/2,30-50/10 0-23/2 * * * root /bin/ls -l /tmp >/dev/null
    2019-08-25 04:03:00 +1'00" (NOW+22'21") /etc/cron.d/test:1:1-5,10-20/2,30-50/10 0-23/2 * * * root /bin/ls -l /tmp >/dev/null
    2019-08-25 04:03:00 +0'00" (NOW+22'21") <USER-CRONTAB>:23:3,4-9,40-55/2 3-12 * * * true
    2019-08-25 04:04:00 +1'00" (NOW+23'21") /etc/cron.d/test:1:1-5,10-20/2,30-50/10 0-23/2 * * * root /bin/ls -l /tmp >/dev/null
    2019-08-25 04:04:00 +0'00" (NOW+23'21") <USER-CRONTAB>:23:3,4-9,40-55/2 3-12 * * * true
    2019-08-25 04:05:00 +1'00" (NOW+24'21") /etc/cron.d/test:1:1-5,10-20/2,30-50/10 0-23/2 * * * root /bin/ls -l /tmp >/dev/null
    2019-08-25 04:05:00 +0'00" (NOW+24'21") <USER-CRONTAB>:23:3,4-9,40-55/2 3-12 * * * true
    2019-08-25 04:06:00 +1'00" (NOW+25'21") <USER-CRONTAB>:23:3,4-9,40-55/2 3-12 * * * true
    2019-08-25 04:07:00 +1'00" (NOW+26'21") <USER-CRONTAB>:23:3,4-9,40-55/2 3-12 * * * true
    2019-08-25 04:08:00 +1'00" (NOW+27'21") <USER-CRONTAB>:23:3,4-9,40-55/2 3-12 * * * true
    2019-08-25 04:09:00 +1'00" (NOW+28'21") <USER-CRONTAB>:23:3,4-9,40-55/2 3-12 * * * true
    2019-08-25 04:10:00 +1'00" (NOW+29'21") /etc/cron.d/test:1:1-5,10-20/2,30-50/10 0-23/2 * * * root /bin/ls -l /tmp >/dev/null
    2019-08-25 04:10:00 +0'00" (NOW+29'21") END

### 自分で指定したファイルのみを読み込む

    % bin/crontab-check --file /etc/crontab --past 30m --in 30m
    2019-08-25 03:27:00 ------ (NOW-30'10") BEGIN
    2019-08-25 03:57:10 ------ (NOW)
    2019-08-25 04:17:00 +19'50" (NOW+19'50") root cd / && run-parts --report /etc/cron.hourly
    2019-08-25 04:27:00 +10'00" (NOW+29'50") END

### 文法エラーもしくはあやしい場合の挙動

#### 例1: 列がたりない

    % cat /tmp/error-example1
    * * * * * root true
    * * *
    % bin/crontab-check --file /tmp/error-example1
    *** Syntax Error: month is missing at /tmp/error-example1:2:* * *

#### 例2: 数値がヘン

    % cat /tmp/error-example2
    2-9/2,30-60 * * * * root true
    % bin/crontab-check --file /tmp/error-example2
    *** Syntax Error: 60 is too large (> 59) at /tmp/error-example2:1:2-9/2,30-60 * * * * root true

#### 例3: 最終行の改行がない

    % cat /tmp/error-example3 && echo
    * * * * * root true
    * * * * * root true
    % bin/crontab-check --file /tmp/error-example3
    *** Syntax Error: NO EOL at /tmp/error-example3:2:* * * * * root true
