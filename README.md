# hkt
Examples for Higher Kinded Types for Haxe. The experimental branch can be found here:

https://github.com/Simn/haxe/tree/higher_kinded_types_reloaded

# Example
```haxe
typedef Mappable<M:Mappable<M>> = {
	public function map <A,B> (f:A->B):M<B>;
}

class Test {
	
	public static function main () {
		var a = [1];
		trace(mapTwice(a, function (x) return x + 1));
	}
	
	static function mapTwice <M:Mappable<M>, A>(x:M<A>, f:A->A) {
		return x.map(f).map(f);
	}

}
```
