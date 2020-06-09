# m2i-jour2
Test commandes de patch GIT



ACTION=$1
NUM=$2

if [ "$NUM" != "1" -a "$NUM" != "2" -a "$NUM" != "3"  ] ; then

	echo "P1:ACTION  : {DESTROY|INIT}"
	echo "P2:NUM : {1|2|3}"
	exit 0

fi

export PGDATA=/dbdata/int$NUM/postgres921/postgres

if [ "$ACTION" == "DESTROY" ] ; then
	pg_ctl --mode=immediate stop
	sleep 5
	rm -rf $PGDATA
fi

if [ "$ACTION" == "INIT" ] ; then
	export PGHOST=localhost
	export PGDATABASE=postgres
	export PGPORT=543$NUM
	export PGUSER=postgres
	export PGPASSWORD=postgres


	initdb -U postgres

	echo "host  all  all  0.0.0.0/0  trust" >>   $PGDATA/pg_hba.conf
	echo "listen_addresses = '*'"           >>   $PGDATA/postgresql.conf
	echo "port = $PGPORT"                   >>   $PGDATA/postgresql.conf

	pg_ctl start
fi


