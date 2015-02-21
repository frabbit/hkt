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
