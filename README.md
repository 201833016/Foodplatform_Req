# Foodplatform_Req
__컴퓨터과학과 백승우__ 의 데이터베이스 과목의 프로젝트의 요구사항 명세서를 적은 장소입니다.

# 요구사항 명세서
*1. 회원 가입을 위하려면 아이디, 비밀번호, 이름을 입력해야한다.

*2. 가입한 회원에게 등급, 저장된 금액, 주문 횟수가 부여된다.

*3. 사용자를 아이디로 식별한다.

*4. 상품에 대한 상품명, 가격, 가게종류 정보를 유지한다.

*5. 상품은 상품명으로 식별한다.

*6. 사용자는 여러 상품을 주문 할수 있으며, 하나의 상품을 여러 사용자가 주문할수 있다.

*7. 회원이 상품을 주문하면 주문에 대한 번호, 아이디, 주문상품, 주문일자 정보를 유지해야 한다.

*8. 각 상품은 가게에서 공급하고, 가게 하나는 여러 상품을 공급할수 있다.

*9. 가게에 대한 가게아이디, 사장이름, 가게명 정보를 유지해야 한다.

*10. 가게정보는 가게 아이디로 식별한다.


# 데이터베이스
* create database foodplatform;

# 사장님
* create table ceo(ceoid varchar(45) not null primary key, ceoname varchar(45) not null, ceoshop varchar(45) not null);

> INSERT INTO ceo VALUES ('ChickenCeo', '닭튀김사장', '치킨');
>
> INSERT INTO ceo VALUES ('CafeCeo', '커피사장', '카페');

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
foodstock int,
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
no int not null
id varchar(45),
product varchar(45),
ymd date,
primary key (no),
foreign key(user) references user.(id),
foreign key(product) references foodmenu.(foodname)
);

# 장바구니
* create table orderlog2(
id varchar(45),
product varchar(45),
ymd date
);

# 종합 수익
create table totalrevenue(
shopname varchar(45) not null,
sellmenu varchar(45) not null primary key,
sellcount int default 0,
totalmoney int default 0
);

> insert into totalrevenue(shopname, sellmenu) values('치킨', '후라이드 치킨');
>
>insert into totalrevenue(shopname, sellmenu) values('치킨', '음료');
>
>insert into totalrevenue(shopname, sellmenu) values('치킨', '치킨소스');
>
>insert into totalrevenue(shopname, sellmenu) values('카페', '아메리카노');
>
>insert into totalrevenue(shopname, sellmenu) values('카페', '카페라떼');
>
>insert into totalrevenue(shopname, sellmenu) values('카페', '조각케잌');
