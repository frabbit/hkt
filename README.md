# hkt
Examples for Higher Kinded Types for Haxe. The experimental branch can be found here:

https://github.com/Simn/haxe/tree/higher_kinded_types_reloaded

# Example
```haxe
typedef Mappable<M:Mappable<M,_>,A> = {
    public function map <B> (f:A->B):M<B>;
}

class Main {
	
	static function main () {
		var a = [1,2];
		var l = new List(); l.add(1); l.add(2);
		
		var a1 = mapTwice(a, function (x) return x + 1);
		var l1 = mapTwice(l, function (x) return x + 1);
		
		$type(a1); // Array<Int>
		$type(l1); // List<Int>
		
		trace(a1);
		trace(l1);
	}
	
	static function mapTwice <M:Mappable<M,_>, A>(x:M<A>, f:A->A) {
		return x.map(f).map(f);
	}
}
```

# Example2
```haxe
typedef Mappable<M:Mappable<M,_>,A> = {
    public function map <B> (f:A->B):M<B>;
}

typedef Filterable<M:Filterable<M,_>,A> = {
    public function filter (f:A->Bool):M<A>;
}

typedef FlatMappable<M:FlatMappable<M,_>,A> = {
    public function flatMap <B> (f:A->M<B>):M<B>;
}

typedef Liftable<M:Liftable<M>> = {
    public function lift <A>(f:A):M<A>;
}

typedef Monad<M:Monad<M,_>,A> = {
  > Liftable<M>,
  > Mappable<M,A>,
  > FlatMappable<M,A>,
}

typedef MonadZero<M:MonadZero<M,_>,A> = {
  > Filterable<M,A>,
  > Monad<M,A>,
}

class MyArray<X> {
  var a : Array<X>;
  public function new (a:Array<X>) {
    this.a = a;
  }

  public function filter (f:X->Bool) {
    return new MyArray(this.a.filter(f));
  }
  
  public function map<Y>(f:X->Y) {
    return new MyArray(this.a.map(f));
  }
  
  public function lift <A>(x:A) {
    return new MyArray([x]);
  }

  
  public function flatMap <A>(f:X->MyArray<A>) {
    var r = [];
    for (x in a) {
      for (y in f(x).a) {
	r.push(y);
      }
    }
    return new MyArray(r);
  }
}

class Main {

    static function main () {
        var a = new MyArray([1,2,3]);
        
        var b = withMonadPlus(a);
        
        trace(b);
    }

    static function withMonadZero <M:MonadZero<M,_>>(m:M<Int>) 
    {
	return m.flatMap(function (x) return m.lift(x+1))
	  .filter(function (x) return x < 3)
	  .map(function (x) return x + 1);
    }
}
```
