# ThreadLoacl #
>其实是一个容器，用来存放线程的局部变量

    public interface Sequence{
		int getNumber();
	}

	public class SequenceByThreadLocal implements Squence{
		private static ThreadLocal<Integer> numberContainer = new ThreadLocal<Integer>(){
			@Override
			protected Integer initialValue(){
				return 0;
			}
		};

		public int getNumber(){
			numberContainer.set(numberContainer.get()+1);
			return numberContainer.get();
		}
	}

通过ThreadLocal封装静态成员变量，保证每一个线程都单独的静态成员变量，保证了线程安全。

ThreadLocal为每一个线程提供了一个独立的副本。

## 接口 ##

	public void set(T value)

	public T get()

	从局部变量中移除值，有助于JYM垃圾回收
	public void remove()

	返回线程局部变量中的初始值
	protected T initialValue() 

## 简易实现 ##

    public class MyThreadLocal<T>{
		private Map<Thread,T> container = Collections.synchronizedMap(new HashMap<Thread,T>());

		public void set(T vaule){
			container.put(Thread.currentThread(),value);
		}

		public T get(){
			Thread thread = Thread.currentThread();
			T value = container.get(thread);
			if(value == null &&　!container.containsKey(key){
				valuee = initValue();
				container.put(thread,value);
			}
			return value;
		}

		public void remove(){
			container.put(Thread.currentThread());
		}

		protected T initialValue(){
			return null;
		}
	}