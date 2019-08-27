# crontab-check

## これなに？

cronをシミュレートして、いつどんなプログラムが実行されるかを一覧表示します。
想定するユースケースは以下です。
* すでに動いているcrontabを、実行されるジョブの時系列で確認する。
  * 既存ジョブとぶつからないように作業したい人向け。
* これから変更するcrontabが、思い通りに書けているかどうか確認する。
  * 開発者やcronの設定変更をしたい人向け。

## 使い方

### 1分以内に開始したジョブと、向こう60分以内に開始する予定のジョブを一覧する

    % bin/crontab-check
    2019-08-26 20:30 --------- (    -0'40" ) BEGIN
    2019-08-26 20:30 --------- (    -0'40" ) root /bin/ls -l /tmp >/dev/null
    2019-08-26 20:30:40 ------ ------------- NOW
    2019-08-26 20:31    +0'20" (    +0'20" ) - /bin/ls -l > /dev/null
    2019-08-26 20:40    +9'00" (    +9'20" ) root /bin/ls -l /tmp >/dev/null
    2019-08-26 20:41    +1'00" (   +10'20" ) - /bin/ls -l > /dev/null
    2019-08-26 20:50    +9'00" (   +19'20" ) root /bin/ls -l /tmp >/dev/null
    2019-08-26 20:51    +1'00" (   +20'20" ) - /bin/ls -l > /dev/null
    2019-08-26 21:01   +10'00" (   +30'20" ) - /bin/ls -l > /dev/null
    2019-08-26 21:11   +10'00" (   +40'20" ) - /bin/ls -l > /dev/null
    2019-08-26 21:17    +6'00" (   +46'20" ) root cd / && run-parts --report /etc/cron.hourly
    2019-08-26 21:21    +4'00" (   +50'20" ) - /bin/ls -l > /dev/null
    2019-08-26 21:30    +9'00" (   +59'20" ) END

### コマンドではなく crontab の定義箇所を出力する

    % bin/crontab-check --print-location
    2019-08-26 20:30 --------- (    -0'40" ) BEGIN
    2019-08-26 20:30 --------- (    -0'40" ) /etc/cron.d/test:1:1-5,10-20/2,30-50/10 0-23/2 * * * root /bin/ls -l /tmp >/dev/null
    2019-08-26 20:30:40 ------ ------------- NOW
    2019-08-26 20:31    +0'20" (    +0'20" ) <USER-CRONTAB>:23:1-59/10 * * * * /bin/ls -l > /dev/null
    2019-08-26 20:40    +9'00" (    +9'20" ) /etc/cron.d/test:1:1-5,10-20/2,30-50/10 0-23/2 * * * root /bin/ls -l /tmp >/dev/null
    2019-08-26 20:41    +1'00" (   +10'20" ) <USER-CRONTAB>:23:1-59/10 * * * * /bin/ls -l > /dev/null
    2019-08-26 20:50    +9'00" (   +19'20" ) /etc/cron.d/test:1:1-5,10-20/2,30-50/10 0-23/2 * * * root /bin/ls -l /tmp >/dev/null
    2019-08-26 20:51    +1'00" (   +20'20" ) <USER-CRONTAB>:23:1-59/10 * * * * /bin/ls -l > /dev/null
    2019-08-26 21:01   +10'00" (   +30'20" ) <USER-CRONTAB>:23:1-59/10 * * * * /bin/ls -l > /dev/null
    2019-08-26 21:11   +10'00" (   +40'20" ) <USER-CRONTAB>:23:1-59/10 * * * * /bin/ls -l > /dev/null
    2019-08-26 21:17    +6'00" (   +46'20" ) /etc/crontab:11:17 *   * * *   root    cd / && run-parts --report /etc/cron.hourly
    2019-08-26 21:21    +4'00" (   +50'20" ) <USER-CRONTAB>:23:1-59/10 * * * * /bin/ls -l > /dev/null
    2019-08-26 21:30    +9'00" (   +59'20" ) END

### システムにインストールされているcrontabを自動でロードするのではなく、特定のファイルだけを指定して確認する

    % bin/crontab-check --file /etc/crontab
    2019-08-26 20:30 --------- (    -0'40" ) BEGIN
    2019-08-26 20:30:40 ------ ------------- NOW
    2019-08-26 21:17   +46'20" (   +46'20" ) root cd / && run-parts --report /etc/cron.hourly
    2019-08-26 21:30   +13'00" (   +59'20" ) END

### その他オプションいろいろ

    % bin/crontab-check --help
    bin/crontab-check - crontab simulator
    usage:
            bin/crontab-check <option-spec>
    option-spec:
            [--past <duration-spec>]
            [--in   <duration-spec>]
                    Show jobs in the past and future.
                    Default: at most 59 seconds in the past, and 60 minutes in the future.
            [--at-unix <UNIXTIME>]
                    Specify timeline origin. Default: now (i.e. the time just before output starts.)
            [--file      <path-spec>] ...
            [--user-file <path-spec>] ...
                    Load specified files instead of installed ones.
                    --file is for system-crontab files (that has user field).
                    --user-file is for user-crontab files (that does not have user field).
            [--pattern <PATTERN>] ...
                    Filter entries: Show the entry only if command has PATTERN as substring.
            [--print-location]
                    Show raw crontab entry and its location, instead of job command.
    duration-spec:
            <num>m          minutes.
            <num>h          hours.
            <num>d          days.
            <num>w          weeks.
    path-spec:
            -               STDIN
            other           Path to the file to load.

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
