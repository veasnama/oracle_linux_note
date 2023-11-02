# How to unlock snapshot stuck in OLVM | Ovirt

- SSH into the OLVM
  ```
  su - postgres
  
  ```
- Connect the engine using psql
```
psql -d engine
```

- Now yo have to get the snapshot_id info from the database, to update it from LOCKED to OK
```
select snapshot_id,description,status from snapshots where status = 'LOCKED' and vm_id in (select vm_guid from vm_static where vm_name='');
```

- Update it to be OK

```
update snapshots set status = 'OK' where snapshot_id = '';

```
- Also you have to check if there are another images locked involved with this snapshot
```
select image_guid,imagestatus from images where vm_snapshot_id = '' and imagestatus != 1;
```
