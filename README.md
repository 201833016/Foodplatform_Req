# Foodplatform_Req
__컴퓨터과학과 백승우__ 의 데이터베이스 과목의 프로젝트의 요구사항 명세서를 적은 장소입니다.

# 요구사항 명세서
1. 회원 가입을 위하려면 아이디, 비밀번호, 이름을 입력해야한다.
2. 가입한 사용자에게 등급, 저장된 금액, 주문 횟수가 부여된다.
3. 사용자를 아이디로 식별한다.

4. 음식에 대한 음식명, 가격, 가게종류 정보를 유지한다.
5. 음식은 음식명으로 식별한다.
6. 사용자는 여러 음식을 주문 할수 있으며, 하나의 상품을 여러 사용자가 주문할수 있다.
7. 사용자가 음식을 주문하면 주문에 대한 아이디, 주문음식, 주문일자 정보를 유지해야 한다.

8. 각 가게는 사장이 운영하고, 사장은 하나의 가게 하나는 운영할수 있다.
9. 가게에 대한 가게아이디, 사장이름, 가게명 정보를 유지해야 한다.
10. 가게정보는 가게 아이디로 식별한다.

11. 쿠폰은 하나씩만 존재하며, 사용자 한명이 여러 쿠폰을 가질 수 없다.
12. 쿠폰이 추첨되어지면 쿠폰아이디, 사용자이름, 할인가격 정보를 유지해야한다.
13. 쿠폰은 쿠폰아이디로 식별한다.

13. 장바구니는 하나의 사용자가 여러개의 등록을 할수 있다.
14. 사용자가 등록을하면 사용자아이디, 주문음식, 주문날짜 정보를 유지해야한다.

15. 주문내역은 하나의 사용자가 여러개의 주문을 할수 있다.
16. 사용자가 주문을하면 주문번호, 사용자아이디, 주문음식, 주문날짜 정보를 유지해야한다.
17. 주문내역은 주문번호로 식별한다.


# 릴레이션 스키마
ceo : ceoid, ceoname, ceoshop

user : id,	password,	name,	money,	grade, 	ordercount

foodmenu : foodname, foodprice, foodmesort

orderlog : no, user, product, ymd, count, shop, TPrice

orderlog2 : user, product, ymd, count, shop, TPrice

coupon : cpid, discount, cpname

eventwin : userid, cpid, grade, discount, eventname

# 데이터베이스
* create database foodplatform;

# 사장님
* create table ceo(ceoid varchar(45) not null primary key, ceoname varchar(45) not null, ceoshop varchar(45) not null);

> INSERT INTO ceo VALUES ('chickenceo', '닭튀김사장', '치킨');
>
> INSERT INTO ceo VALUES ('cafeceo', '커피사장', '카페');

# 사용자
* create table user(
id varchar(45) not null,
password varchar(45) not null,
name varchar(45) not null,
money int default 0,
grade varchar(45) not null default '브론즈',
ordercount int default 0,
primary key (id)
);

>INSERT INTO user(id, password, name) VALUES ('kim', 'q1w2', '김승주');
>
>INSERT INTO user(id, password, name) VALUES ('lee', 'e3r4', '이강선');
>
>INSERT INTO user(id, password, name) VALUES ('park', 'zx56', '박유현');

# 음식종류
* create table foodmenu(
foodname varchar(45) not null,
foodprice int,
foodmesort varchar(45),
primary key (foodname)
);

>INSERT INTO foodmenu VALUES ('아메리카노', 3500, '카페');
>
>INSERT INTO foodmenu VALUES ('카페라뗴', 4500, '카페');
>
>INSERT INTO foodmenu VALUES ('조각케잌', 6000, '카페');
>
>INSERT INTO foodmenu VALUES ('후라이드 치킨', 16000, '치킨');
>
>INSERT INTO foodmenu VALUES ('음료', 2500, '치킨');
>
>INSERT INTO foodmenu VALUES ('치킨소스', 1000, '치킨');

# 주문내역
* create table orderlog(
no int not null auto_increment,
user varchar(45),
product varchar(45),
ymd date,
count int,
shop varchar(45),
TPrice int default 0,
primary key (no),
foreign key(user) references user.(id),
foreign key(product) references foodmenu.(foodname)
);

# 장바구니
* create table orderlog2(
user varchar(45),
product varchar(45),
ymd date,
shop varchar(45),
TPrice int default 0
);


# 쿠폰
create table coupon(
cpid varchar(45) not null,
discount int not null,
cpname varchar(45),
primary key(cpid)
);

> insert into coupon values('cp01', 1000, '1000원할인');
> 
>insert into coupon values('cp02', 3000, '3000원할인');
>
>insert into coupon values('cp03', 5000, '5000원할인');

# 소유한 쿠폰
create table eventwin(
userid varchar(45) not null,
cpid varchar(45),
grade varchar(45),
discount int,
eventname varchar(45),
primary key(userid)
);

# 테이블 정규화
제1정규형
[table] eventwin
- userid에서 kim이 쿠폰을 2개를 가지고 있어 중복.
- 제1정규형에 속하게 하기위해, 투플마다 cpid 속성값을 1개씩만 포함하도록, 분해, 모든 속성이 원자 값을 가지도록 한다.


제2정규형
[table] eventwin
- 부분 함수 종속을 제거하고 모든 속성이 기본키에 완전 함수 종속이 되도록 릴레이션을 분해.
- eventwin 릴레이션을 부분 함수 종속을 제거하려고 분해
- -> userid, grade [사용자 릴레이션] 
- -> cpid, eventname, discount [쿠폰 릴레이션]

제3정규형
- 모든 속성이 기본키에 이행적 함수 종속이 되지 않도록 분해
- 릴레이션 구성의 속성집합X,Y,Z에대해  X->Y, Y ->Z가 존재하면 논리적으로 X->Z가 성립, 이때 "Z는 X에 이행적으로 종속되었다" 라고 함.
- 이행적 함수 종속 일어나지 않도록 분해

- [쿠폰 릴레이션] 분해
- -> cpid, eventname [쿠폰 명 릴레이션]
- -> eventname, discount [쿠폰 가격 릴레이션]

BCNF
[table] orderlog2
- 1개 릴레이션에 여러개의 후보키가 존재하는 경우 ,제 3 정규형 까지 모두 만족해도 이상 현상이 발생 할 수 있다.
- user, foodmenu [주문자 - 메뉴 릴레이션]
- foodmenu, shop[메뉴 - 가게 릴레이션]

