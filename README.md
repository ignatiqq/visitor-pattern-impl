# visitor-pattern-impl

Мы хотим иметь возможность определять новые операции с выпечкой — приготовление, употребление в пищу, украшение и т. д.
без необходимости каждый раз добавлять новый метод в каждый класс. Вот как мы это делаем. Сначала мы определяем отдельный интерфейс.

```
interface PastryVisitor {
    visitBeignet(beignet: Beignet) => void; 
    visitCruller(cruller: Cruller) => void;
}

abstract class Pastry {
  abstract accept(visitor: PastryVisitor) => void;
}

```

**КАЖДАЯ ОПЕРАЦИЯ**, которую можно выполнить с выпечкой, представляет собой **НОВЫЙ КЛАСС**, реализующий этот интерфейс.
Для каждого вида выпечки существует конкретный метод. Благодаря этому код операции над обоими типами плотно прилегает друг к другу в одном классе.

```
class Beignet extends Pastry {
  taste = 'yammy';
  name = 'beignet';
  
  accept(visitor: PastryVisitor) {
    visitor.visitBeignet(this);
  }
}

class Cruller extends Pastry {
  taste = 'bad';
  name = 'cruller';
  
  accept(visitor: PastryVisitor) {
    visitor.visitCruller(this);
  }
}
```

Чтобы выполнить операцию с выпечкой, мы вызываем его accept()метод и передаем посетителю операцию, которую хотим выполнить. 
**Выпечка — переопределяющая реализацию конкретного подкласса accept()— разворачивается, вызывает соответствующий метод посещения посетителя и передает себя ему. (Cruller передает себя в visitor.visitCruller со всеми данными)**
Вот в этом суть трюка. Это позволяет нам использовать полиморфную диспетчеризацию в классах кондитерских изделий , чтобы выбрать соответствующий метод в классе посетителей


дополняем функциональность "Pastry" классов с помощью нового класса, а не определения общего метода в их теле
тоесть этот класс мы можем использовать сразу для всех (определенных) выпечек

```
class BakePastry: implements PastryVisitor {
  visitCruller(cruller: Cruller) {
    console.log('baking cruller');
    // manipulate with cruller data
  }

  visitBeignet(beignet: Beignet) {
    console.log('baking beignet');
    // manipulate with cruller data
  }
}

const bake = new BakePastry();
const cruller = new Cruller();
cruller.accept(bake);
// 'baking cruller'
```


