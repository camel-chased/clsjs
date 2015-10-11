# clsjs
javascript classes with protected, private, public, static declarations, with runtime type checking and default function arguments.


## usage

```js

var classContructor = cls('someClass',function(){return {

    /**
     * @method someMethod public
     * @param {string} name = 'defaultName'
     * @returns {undefined}
     */
    someMethod:function someMethod(name){
        console.log(name);
    }

};});

var classInstance = new classConstructor();

classInstance.someMethod();         // logs 'defaultName'
classInstance.someMethod('john');   // logs 'john'
classInstance.someMethod(23);       // type mismatch error thrown

```

### extending

```js

var extConstr=cls('someExtension',classConstructor,function(){return{

    // by default methods are public
    /**
     * [someOtherFn description]
     * @method  someOtherFn
     * @returns {undefined} [description]
     */
    someOtherFn:function someOtherFn(){
        return this.someMethod('yeaaah');
    }

};});

var extInstance = new extConstr();
extInstance.someOtherFn();          // logs 'yeaaah'

```

### private

Private properties/methods are not inherited and cannot access them outside a class instance.

```js
var clsPrv = cls('clsPrv',function(){return{

    /** @property {string} privateProp private */
    privateProp:'hello world',

    /**
     * [privateFn description]
     * @method  privateFn private
     * @returns {string} some string telling something...
     */
    privateFn:function privateFn(){
        return 'this is private fn that cannot be accessed anywhere else outside this instance';
    },


    publicFn:function publicFn(){
        return this.privateFn();
    }

};});

var prvInstance = new clsPrv();
prvInstance.privateFn();            //throws error
prvInstance.publicFn();             //return string 'this is....'

var prvExt = cls('prvExt',clsPrv,function(){return{

    publicTest:function publicTest(){
        return this.privateFn();
    }

};});

var extInstance = new prvExt();
extInstance.publicTest();           // throws error because of privateFn does not exist in this instance
```

### protected

protected properties/methods are visible only within a class instance and are inherited to child classes
you cannot access protected variables outside a class instance
