# Design Ticketmaster

## Scenario
1. book ticket
2. auto-assign seat, user-pick seat

## Suppose
1. Write QPS, 10,000
2. read-write ratio, 100:1
3. total seat, 100,000

## Service
### UserService#login(Credential credential);
return jwt-token if login success

### TicketService#count(Zone zone);
available seat by zone

### TicketService#book(Zone zone, Seat seat);
book ticket by zone and seat

### BlackService#add(Credential credential);
block abnormal request who exceed threshold

## Storage
credentail-cache, jwt-token, ttl:10m
zone-{a,b,c,..,n}-seat-cache, taken-seat
black-list-cache, user-access

```mysql
CREATE TABLE IF NOT EXISTS `seat` (
`id` INT UNSIGNED AUTO_INCREMENT NOT NULL,
`zone_id` VARCHAR(40) DEFAULT NULL,
`column` VARCHAR(40) DEFAULT NULL,
`row` VARCHAR(40) DEFAULT NULL,
`user_id` VARCHAR(40) DEFAULT NULL, 
`create_time` DATETIME NOT null, PRIMARY KEY (`id`) ); 
```

## Evolve
1. Diversion
2. Rate-limit
3. Cache
4. Async

**Reference**

1. https://gitee.com/52itstyle/spring-boot-seckill
2. https://medium.com/@hlb/kktix-2015-01-7bf84c47dfdf
