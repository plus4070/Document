```
#!/bin/sh
. ~/.bashrc
SCRIPT_DIR='/home1/scripts/'
function messageSender(){
  baseMessenger=$SCRIPT_DIR'message-Sender.sh '
  messageSender=$baseMessenger$1
  $messageSender
}
function restartServer(){
  RESTART='/home1/scripts/webapps.sh restart'
  $RESTART
}

#CPU_USAGE='mpstat | tail -1 | awk \'{print 100-$11}\''
MEMORY=`free | sed '2p' -n | awk '{printf"%.0f", $3 * 100/( $3 + $4)}'`
#MEMORY_USAGE=${MEMORY_USAGE%%.*}

if [ "$MEMORY" -gt 80 ];
        then
        echo 'Message will be sended'
        messageSender
        restartServer
fi

```

MEMORY= 부분은 `를 안붙이면 스트링으로 인식해서 숫자와 비교를 못한다.
