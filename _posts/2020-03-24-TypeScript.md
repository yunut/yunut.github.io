---
layout: post
title:  "TypeScript_BlockChain"
date:   2020-03-24
excerpt: ""
tag:
- TypeScript
- practice
comments: true
---

## TypeScript
Typed 언어, 어떤 종류의 변수와 데이터인지 설정을 해줘야한다.
  
![tsfeature](/photo/typescript/4.typescriptfeature.PNG)  
자바스크립트에서 지정한 인자수가 적어도 실행되던 것이 타입스크립트에선 에러메시지 발생 -> 더 안전한 프로그래밍 가능

![tsfeature](/photo/typescript/5.tsfeature.PNG) 
또한 선택적으로 인자를 지정가능 

### 설치명령어
yarn global add typescript

### 설정
![tsconfig](/photo/typescript/1.tsconfig.PNG)  
tsconfig.json 파일 생성  
 TypeScript에게 어떻게 자바스크립트로 변환하는지 알려주는 파일
 module : nodejs 사용  
 target : 어느버전의 자바스크립트를 사용할지를 입력
 sourceMap : 소스맵처리  

 include : 컴파일과정에서 포함할 파일

### 컴파일
* tsc 명령어
ts파일에 있는 코드를 컴파일, js파일과 map파일 생성


* yarn start
![yarnconfig](/photo/typescript/2.yarnconfig.PNG)  
yarn start 스크립트로 컴파일, 파일실행까지 만드는 소스 작성

![yarnconfig](/photo/typescript/3.yarnstart.PNG)  
실행결과


### tsc-watch
* 파일변경시 자동으로 컴파일 후 node작업을 해주는 모듈
  
설치명령어 : yarn add tsc-watch --dev  

![tsc-watch](/photo/typescript/6.tsc-watch.PNG)  
tsc-watch script 명령어 설정. 컴파일 성공시 dist/ 폴더로 output
  
![dirchange](/photo/typescript/6.dirchange.PNG)  
컴파일 디렉토리와 output 디렉토리 변경


### 인터페이스 정의
자바스크립트와 다르게 타입스크립트는 인터페이스를 만들어 오브젝트형식을 지정하는게 가능  
인터페이스는 js로 컴파일되지 않는다.  

![interface](/photo/typescript/8.interface.PNG)  



### 클래스
인터페이스를 js로 넣고싶을때 클래스를 사용.
react, express 등을 사용하려고할때 클래스를 사용  

  
![class]](/photo/typescript/9.class.PNG)  



### TypeScript를 활용한 블록체인 만들기
crypto-js 추가 후 작성  
  
소스코드
~~~
import * as CryptoJS from "crypto-js";

class Block {
    static calculateBlockHash = (
        index:number, 
        previousHash:string, 
        timestamp:number, 
        data:string
        ):string => CryptoJS.SHA256(index + previousHash + timestamp + data).toString();

    static validateStructure = (aBlock: Block) : boolean =>
     typeof aBlock.index === "number" && 
     typeof aBlock.hash === "string" && 
     typeof aBlock.previousHash === "string" &&
     typeof aBlock.timestamp === "number" &&
     typeof aBlock.data === "string";


    public index:number;
    public hash:string;
    public previousHash:string;
    public data:string;
    public timestamp:number;

    constructor(
        index:number,
        hash:string,
        previousHash:string,
        data:string,
        timestamp:number
    ) {
        this.index = index;
        this.hash = hash;
        this.previousHash = previousHash;
        this.data = data;
        this.timestamp = timestamp;
    }
}



const genesisBlock:Block = new Block(0, "asdfasdf", "", "start", 123456);

let blockchain: Block[] = [genesisBlock];

const getBlockcahin = () : Block[] => blockchain;

const getLatestBlock = () : Block => blockchain[blockchain.length-1];

const getNewTimeStamp = () : number => Math.round(new Date().getTime() / 1000)

const createNewBlock = (data:string) : Block => {
    const previousBlock : Block = getLatestBlock();
    const newIndex : number = previousBlock.index + 1;
    const newTimestamp : number = getNewTimeStamp();
    const newHash : string = Block.calculateBlockHash(
        newIndex, 
        previousBlock.hash,
        newTimestamp,
        data
    );
    const newBlock : Block = new Block(
        newIndex,
        newHash,
        previousBlock.hash,
        data,
        newTimestamp
    );
    addBlock(newBlock);
        return newBlock;
};

const getHashforBlock = (aBlock: Block) :string => Block.calculateBlockHash(aBlock.index, aBlock.previousHash, aBlock.timestamp, aBlock.data)

const isBlockVaild = (candidateBlock : Block, previousBlock: Block): boolean => {
    if(!Block.validateStructure(candidateBlock)) {
        return false;
    } else if(previousBlock.index + 1 !== candidateBlock.index) {
        return false;
    } else if(previousBlock.hash !== candidateBlock.previousHash) {
        return false;
    } else if(getHashforBlock(candidateBlock) !== candidateBlock.hash) {
        return false;
    } else {
        return true;
    }
};

const addBlock = (candidateBlock: Block) : void => {
    if(isBlockVaild(candidateBlock, getLatestBlock())) {
        blockchain.push(candidateBlock);
    }
};

createNewBlock("second block");
createNewBlock("third block");
createNewBlock("forth block");

console.log(blockchain);
export {}
~~~




