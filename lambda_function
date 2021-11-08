import boto3
import logging
import time

region = 'ap-northeast-1'
instance = 'database-1'
rds = boto3.client('rds', region_name=region)
LOGGER = logging.getLogger()
LOGGER.setLevel(logging.INFO)


def lambda_handler(event, context):
    count = 0
    failcount = 0
    
    while count < 5:
        LOGGER.info(event)
        db_instances = rds.describe_db_instances(DBInstanceIdentifier=instance)['DBInstances']
        db_instance = db_instances[0]
        rdsInstanceStatus = db_instance['DBInstanceStatus']
        #カウント追加
        if rdsInstanceStatus == 'available':
            count += 1
            print('counts number:' + str(count))
            time.sleep(60) 

        elif rdsInstanceStatus == 'backing-up':
            count = 0
            print('instance status:' + rdsInstanceStatus)
            print('counts reset, No failure counts.') 
            #test
            print('failcount:' + str(failcount))
            time.sleep(60) 

        else:
            count = 0
            print('instance status:' + rdsInstanceStatus)
            print('counts reset')
            failcount += 1
            #test
            print('failcount:' + str(failcount))

            if failcount == 5:
                LOGGER.warning("MODE :Error")
                raise Exception("Inner Error")
            time.sleep(60) 
    else:
        rds.stop_db_instance(DBInstanceIdentifier=instance)
        print('stoped instance:' + instance)
        print(event)

        
