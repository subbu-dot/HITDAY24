# HITDAY24

package hit.day24;
public class TwoThreadsOneTask {
	public static void main(String[] args) {
		Canon bofors=new Canon();
		ShootingTask st=new ShootingTask(bofors);
		
		Thread naina=new Thread(st,"naina");
		Thread shabeer=new Thread(st,"shabeer");
		
		naina.start();
		shabeer.start();
	}
}
class ShootingTask implements Runnable{
	Canon gun;
	public ShootingTask(Canon gun) {
		this.gun=gun;
	}
	@Override
	public void run() {
		Thread t=Thread.currentThread();
		if(t.getName().equals("naina")) {
			for(int i=0;i<5;i++) {
				gun.fill();
			}
		}
		else if(t.getName().equals("shabeer")) {
			for(int i=0;i<5;i++) {
				gun.shoot();
			}
		}
	}
}
class Canon{
	public void fill() {
		Thread t=Thread.currentThread();
		String name=t.getName();
		System.out.println(name+" fills the gun.......");
	}
	
	public void shoot() {
		Thread t=Thread.currentThread();
		String name=t.getName();
		System.out.println(name+" shoot the gun...........");
	}
}

package hit.day24;
public class TwoThreadDemo {
	public static void main(String[] args) {
		Canon bofors=new Canon();
		ShootingTask st=new ShootingTask(bofors);
		
		Thread naina=new Thread(st,"naina");
		Thread shabeer=new Thread(st,"shabeer");
		
		naina.start();
		shabeer.start();
	}
}
class ShootingTask implements Runnable{
	Canon gun;
	public ShootingTask(Canon gun) {
		this.gun=gun;
	}
	@Override
	public void run() {
		Thread t=Thread.currentThread();
		if(t.getName().equals("naina")) {
			for(int i=0;i<5;i++) {
				gun.fill();
			}
		}
		else if(t.getName().equals("shabeer")) {
			for(int i=0;i<5;i++) {
				gun.shoot();
			}
		}
	}
}
class Canon{
	boolean flag;
	synchronized public void fill() {
		Thread t=Thread.currentThread();
		String name=t.getName();
		if(flag) {
			try{wait();}catch(Exception e) {}
		}
		System.out.println(name+" fills the gun.......");
		flag=true;
		notify();
	}
	
	synchronized public void shoot() {
		Thread t=Thread.currentThread();
		String name=t.getName();
		if(!flag) {
			try {wait();}catch(Exception e) {}
		}
		System.out.println(name+" shoot the gun...........");
		flag=false;
		notify();
	}
}

package hit.day24;
public class ThreadRevision {
	public static void main(String[] args) {
		Canon bofors=new Canon();
		ShootingTask st=new ShootingTask(bofors);
		
		Thread naina=new Thread(st,"naina");
		Thread shabeer=new Thread(st,"shabeer");
		
		naina.start();
		shabeer.start();
	}
}
class ShootingTask implements Runnable{
	Canon gun;
	public ShootingTask(Canon gun) {
		this.gun=gun;
	}
	@Override
	public void run() {
		Thread t=Thread.currentThread();
		if(t.getName().equals("naina")) {
			for(int i=0;i<5;i++) {
				gun.fill();
			}
		}
		else if(t.getName().equals("shabeer")) {
			for(int i=0;i<5;i++) {
				gun.shoot();
			}
		}
	}
}
//wait and notify can be used only inside a monitor
//monitor means, when you create a synchronized block (either pesimistic or optimistic), u call it as monitor
//when u call WAIT on a thread inside a monitor, remember another thread CAN enter the monitor
//whereas when u call SLEEP on a thread inside a monitor, remember another thread CANNOT enter the monitor.
class Canon{
	boolean flag;
	synchronized public void fill() {
		Thread t=Thread.currentThread();
		String name=t.getName();
		if(flag) {
			try{wait();}catch(Exception e) {}
		}
		System.out.println(name+" fills the gun.......");
		flag=true;
		notify();
	}
	
	synchronized public void shoot() {
		Thread t=Thread.currentThread();
		String name=t.getName();
		if(!flag) {
			try {wait();}catch(Exception e) {}
		}
		System.out.println(name+" shoot the gun...........");
		flag=false;
		notify();
	}
}

package hit.day24;
public class DeadLockDemo {
	public static void main(String[] args) {
		Frog frog=new Frog();
		Crane crane=new Crane();
		new Thread(new Runnable() {			
			@Override
			public void run() {
				crane.eat(frog);
			}
		}).start();
		
		new Thread(new Runnable() {			
			@Override
			public void run() {
				frog.escape(crane);
			}
		}).start();
	}
}
class Crane{
	synchronized public void eat(Frog frog) {
		System.out.println();
		frog.leaveCallByCrane();
	}
	
	synchronized public void leaveCalledByFrog() {
		
	}
}
class Frog{
	synchronized public void escape(Crane crane) {
		crane.leaveCalledByFrog();
	}
	
	synchronized public void leaveCallByCrane() {
		
	}
}
