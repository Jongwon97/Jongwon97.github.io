---
layout: single
title: "Genetic algorithm"
---

### (Genetic algorithm)을 이용한 회기식 찾기

###### -201601726 이종원

---

**[사용한 데이터]**: 8~19세 의 몸무게,키를 모아둔 데이터에서 약100명의 학생만 선택하여 사용했습니다. (남자, 나이별로 일정 퍼센트 선택)

* 어떤 형태의 그래프가 나올지 확인을 위해 우선 데이터를 그래프에 나타내었습니다.

![image](https://user-images.githubusercontent.com/62750326/85813673-d38a6680-b79e-11ea-8de4-6ac14617ad27.png)  

* 그래프에서 볼 수 있듯이 함수는 y=ax+b 형태로 예상하고 시작했습니다.

##### [a,b 계수 구하기 matlab]

~~~ matlab
weight=[27.3000000000000;25.4000000000000;23.5000000000000;20;33.5000000000000;24;29.6000000000000;26.8000000000000;38.3000000000000;38.1000000000000;31.6000000000000;29.6000000000000;30.5000000000000;36.3000000000000;22.8000000000000;30.4000000000000;29.1000000000000;37.5000000000000;41.3000000000000;52.2000000000000;27.3000000000000;25.4000000000000;45;37.5000000000000;32.2000000000000;36.7000000000000;34.4000000000000;34.1000000000000;35.3000000000000;34.2000000000000;35.5000000000000;48.8000000000000;30.8000000000000;51.5000000000000;61.1000000000000;61.5000000000000;44.5000000000000;39.8000000000000;40.2000000000000;45.6000000000000;32.4000000000000;36.8000000000000;48.3000000000000;46.3000000000000;47.2000000000000;39.5000000000000;38.1000000000000;45.6000000000000;50.1000000000000;52.9000000000000;89.8000000000000;51.1000000000000;73.9000000000000;58.5000000000000;75.1000000000000;75.1000000000000;65.5000000000000;56.2000000000000;58.1000000000000;77.2000000000000;51.7000000000000;76.5000000000000;65.8000000000000;95;55.9000000000000;69.5000000000000;60.5000000000000;61.1000000000000;46.7000000000000;52.6000000000000;53.2000000000000;51.6000000000000;73.3000000000000;88.2000000000000;96.6000000000000;74.7000000000000;70.8000000000000;59.2000000000000;54.4000000000000;64.2000000000000;56.5000000000000;58.8000000000000;65.1000000000000;58.6000000000000;70;70.9000000000000;52.9000000000000;87.2000000000000;62;67;88.3000000000000;77.8000000000000;74.8000000000000;63;55;59;70.1000000000000;60.7000000000000;81.8000000000000;54.6000000000000;70.3000000000000;69.6000000000000;74.8000000000000;57.3000000000000;70.3000000000000;63.5000000000000;70.3000000000000];
high=[125.800000000000;124.300000000000;119.200000000000;115;120;123.600000000000;128.600000000000;126.600000000000;131.900000000000;135.900000000000;128.600000000000;134.400000000000;140.400000000000;140.200000000000;128.100000000000;132.200000000000;134;143.300000000000;133.200000000000;145.300000000000;125.600000000000;127.300000000000;141;133.500000000000;130.400000000000;140.100000000000;137.100000000000;140.200000000000;141.700000000000;144.900000000000;145.600000000000;148.800000000000;143;146.500000000000;153;159.800000000000;147.700000000000;150.700000000000;154.100000000000;143.500000000000;149.300000000000;144.300000000000;144.500000000000;143.400000000000;154.700000000000;161.800000000000;150.900000000000;149.900000000000;162;160.400000000000;167.500000000000;155.300000000000;164.900000000000;166.400000000000;165.200000000000;158.900000000000;170.700000000000;167.400000000000;169.800000000000;172.300000000000;160.900000000000;169.300000000000;173.400000000000;178.700000000000;166;175.400000000000;170.500000000000;169.800000000000;168.600000000000;166.100000000000;174.900000000000;168.400000000000;161;166.500000000000;176.100000000000;180.800000000000;173.700000000000;163.800000000000;169.500000000000;163.500000000000;170.500000000000;173;176.800000000000;171.300000000000;173.400000000000;173.800000000000;172.200000000000;171;171.400000000000;169.600000000000;174.800000000000;172;177.100000000000;163.600000000000;172.900000000000;169.700000000000;170.200000000000;165.200000000000;174.800000000000;167.300000000000;172.500000000000;172.100000000000;171.500000000000;177.300000000000;180.600000000000;177.800000000000;172.300000000000];
n=length(weight);

s_x=sum(weight);s_y=sum(high);
s_x2=sum(weight.^2);
s_xy=sum(weight.*high);

M1=[s_x2 s_x
    s_x n];
M2=[s_xy; s_y];
anss=inv(M1)*M2;
a = anss(1)
b = anss(2)
~~~

[**a=0.8272, b=112.1402**]

* 유전자알고리즘으로 a를 예측할 예정이기 때문에, b값은 미리 112정도로 가정하고 시작했습니다.

**y=ax+112**

---

#### 코드 구현과정

>1. 먼저 랜덤한 a값 4개를 생성합니다. (범위 0.1~10 실수형)
>
>  (아래 반복)
>
>1. 선택연산(selection)으로 a값중에서 다시 4개를 선택합니다.(중복가능)
>2. 정수형으로 바꿔서 이진수 계산을 해야하므로 각각의 실수값에 X100을 하고 정수형으로 변환합니다. (나중에 다시 나눔)
>3. a값들을 전부 이진수로 바꾼뒤 crossover 연산을 합니다.
>
>   4.돌연변이연산(mutation)을 한뒤 다시 다시 10진수로 바꾸고, 정수형을 100으로 나눠서 다시 실수형으로 변환시켜줍니다.
>
>5. 데이터의 평균 키(y)의  값과 후보 a 4개를 각각 넣어서 각각 만든 y값의 차이가 가장 적은 값을 구합니다.(절대값으로 변환)
>
>6. 차이가 가장 적은 값을 가진 후보a를 맨처음 랜덤값 a배열의 0인덱스에 넣어줍니다.
>
>  > **가장 적합한 a값을 a[0]에 넣을것이기 때문에, 반복 해서 더 좋은 값이 나올때마다 0인덱스 값이 바뀌고, 연산과정에서 0인덱스의 값의 변형을 방지하기 위해 selection, crossover, mutation 연산시 a[0]은 제외하였습니다.**



---

#### [코드]

~~~ java
package algorithm;
import java.util.*;

public class Main {
	// 랜덤으로 a값 4개생성
	 public static double[] arand() {
	        Random r = new Random();
	        double[] a1 = new double[4];

	        for(int i=0; i<a1.length; i++) {
	            a1[i] = r.nextFloat()*10+0.1;	            
	        }	       
	        return a1;
	 }
	 
	 // 평균 몸무게값을 이용해서 키 구하기
	    public static double[] fx(double[] a,double w_average) {
	    	double[] y1=new double[4];
	    	int b=112;
	    	
	        for(int i=0; i<a.length;i++) {
	        	y1[i]=a[i]*w_average+b;	        	
	        }
	        return y1;
	    }
//적합도 구하기(원반면적)
	    public static double[] selection(double[] a,double[] y1) {
	    	double sum=0;
	    	for(int i=1; i<y1.length;i++) {
	    		sum=+y1[i];
	    	}
	    	double[] ratio=new double[3];
	    	for(int i=0; i<ratio.length-1;i++) {
	            if(i==0) 
	            	ratio[i] = (double)y1[i] / (double)sum;
	            else 
	            	ratio[i] = ratio[i-1] + (double)y1[i] / (double)sum;
	    	}
	    	double[] a1=new double[4];
	    	Random r=new Random();
	    	a1[0]=a[0];
	    	for(int i=1; i<a1.length;i++) {
	    		double e=r.nextDouble();
	            if(e < ratio[0])
	            	a1[i] = a[1];
	            else if(e < ratio[1])
	            	a1[i] = a[2];
	            else if(e < ratio[2])
	            	a1[i] = a[3];	           	            		    			
	    	}
	    	return a1;
	    }

	    //계산하기위해 정수형 변환
	    public static int[] intChanging(double[] a1) {
	    	int[] a2=new int[a1.length];
	    	for(int i=0;i<a1.length;i++) {
	    		a1[i]=a1[i]*100;
	    		a2[i]=(int)a1[i];
	    		a1[i]=a1[i]/100;
	    	}
	    	return a2;
	    }
	    //크기를10으로 설정하고 앞에 빈곳은 0으로 채운다.
	    public static String int2String(String x) {
	        return String.format("%10s", x).replace(' ','0');
	    }
	    // crossover 연산
	    public static String[] crossOver(int[] a2) {
	    	String[] a3=new String[a2.length];
	    	a3[0]=int2String(Integer.toBinaryString(a2[0]));
	    	
	    		String bit1 = int2String(Integer.toBinaryString(a2[1]));          
	            String bit2 = int2String(Integer.toBinaryString(a2[2]));
	            String bit3 = int2String(Integer.toBinaryString(a2[3]));
	            a3[1]=  bit3.substring(0,3) + bit1.substring(3, 10);
	            a3[2] = bit1.substring(6,10) + bit2.substring(0, 6);
	            a3[3] = bit2.substring(4, 10)+bit3.substring(0, 4);
	    		    	
	    	return a3;
	    }
	    
	 //돌연변이 생성 
	    public static double[] mutation(String[] a3) {
	    	String[] a4=new String[a3.length];	    	
	    	a4[0]=a3[0];
	    	a4[1]=a3[1].substring(0,3)+a3[1].substring(3,4).replace('1','0')+a3[1].substring(4,10);
	    	a4[2]=a3[2].substring(0,5)+a3[2].substring(5,6).replace('0','1')+a3[2].substring(6,10);		
	    	a4[3]=a3[3].substring(0,7)+a3[3].substring(7,10).replace('1','0');		

	    	//다시 10진수로 변환
	    	int[] a5=new int[a4.length];
	    	for(int i=0;i<a4.length;i++) {
	    		a5[i]=Integer.parseInt(a4[i],2);
	    	}	    	
	    	//다시 실수로 만들기
	    	double[] a6=new double[a5.length];
	    	for(int i=0;i<a5.length;i++) {
	    		a6[i]=(double)a5[i]/100;
	    	}
	    	return a6;	    	
	    }
	    //평균값과 가장 가까운 값 찾기
	    public static double fit(double[] a4,double w,double h) {
	    	double ans=0;
	    	double[] y2=new double[a4.length];
	    			y2=fx(a4,w);
	    	double[] abs= {0,0,0,0};
	    	//평균 키와 가장 가까운 값찾기 절대값
	        for(int i=0; i<a4.length;i++) {
	    	abs[i]=((y2[i]-h)<0)?-(y2[i]-h):(y2[i]-h);
	    		         }
	        double min=abs[0];
	        for(int i=1;i<a4.length;i++) {
	        if(abs[i]<min)
	        	min=abs[i];
	        }
	        int q=0;
	        for(int i=0;i<a4.length;i++) {
	        	if(abs[i]==min)
	        		q=i;
	        }
	        ans=y2[q];	
	    	return ans;
	    }
    //가장 적합한 y의 a값 반환
	    public static double fx2(double ans,double w) {
	  	 return (ans-112)/w;
	    }
	    
	public static void main(String[] args) {
		double[] a=arand();
		// 연산을 위해 데이터의 평균 몸무게,키 입력
		double w_average=53.07;
		double h_average=156.04;
		double ar=0;

		for(int i=0;i<100;i++) {
			double[] y1=fx(a,w_average);
			double[] a1=selection(a,y1);
		int[] a2=intChanging(a1);
		String[] a3=crossOver(a2);
		double[] a4=mutation(a3);
		double ans=fit(a4,w_average,h_average);
		double a6=fx2(ans,w_average);
		
		for(int j=0; j<4;j++) {
			System.out.printf("%.2f ", a4[j]);
        }
		System.out.println("");
		System.out.printf("%.2f ",a6);
		System.out.println("");
		y1=null;
		a1=null;
		a2=null;
		a3=null;
		a4=null;
		a[0]=a6;
		ar=a6;

	}
		System.out.printf("y=%.2fx+112",ar);
}	
	
	}
~~~



#### 코드 실행화면

<img src="https://user-images.githubusercontent.com/62750326/85817800-01c17380-b7aa-11ea-93cc-1d62308ad474.PNG" alt="code1" style="zoom: 67%;" /> 



* **genetic algorithm의 결과물인 함수 y=0.89x+112와 데이터를 matlab으로 구현하였습니다.**

---

### [결과]

<img src="https://user-images.githubusercontent.com/62750326/85828864-435f1800-b7c4-11ea-9d33-255b88a60a75.png" alt="image" style="zoom:80%;" /> 

**[코드]**

~~~ matlab
weight=[27.3000000000000;25.4000000000000;23.5000000000000;20;33.5000000000000;24;29.6000000000000;26.8000000000000;38.3000000000000;38.1000000000000;31.6000000000000;29.6000000000000;30.5000000000000;36.3000000000000;22.8000000000000;30.4000000000000;29.1000000000000;37.5000000000000;41.3000000000000;52.2000000000000;27.3000000000000;25.4000000000000;45;37.5000000000000;32.2000000000000;36.7000000000000;34.4000000000000;34.1000000000000;35.3000000000000;34.2000000000000;35.5000000000000;48.8000000000000;30.8000000000000;51.5000000000000;61.1000000000000;61.5000000000000;44.5000000000000;39.8000000000000;40.2000000000000;45.6000000000000;32.4000000000000;36.8000000000000;48.3000000000000;46.3000000000000;47.2000000000000;39.5000000000000;38.1000000000000;45.6000000000000;50.1000000000000;52.9000000000000;89.8000000000000;51.1000000000000;73.9000000000000;58.5000000000000;75.1000000000000;75.1000000000000;65.5000000000000;56.2000000000000;58.1000000000000;77.2000000000000;51.7000000000000;76.5000000000000;65.8000000000000;95;55.9000000000000;69.5000000000000;60.5000000000000;61.1000000000000;46.7000000000000;52.6000000000000;53.2000000000000;51.6000000000000;73.3000000000000;88.2000000000000;96.6000000000000;74.7000000000000;70.8000000000000;59.2000000000000;54.4000000000000;64.2000000000000;56.5000000000000;58.8000000000000;65.1000000000000;58.6000000000000;70;70.9000000000000;52.9000000000000;87.2000000000000;62;67;88.3000000000000;77.8000000000000;74.8000000000000;63;55;59;70.1000000000000;60.7000000000000;81.8000000000000;54.6000000000000;70.3000000000000;69.6000000000000;74.8000000000000;57.3000000000000;70.3000000000000;63.5000000000000;70.3000000000000];
high=[125.800000000000;124.300000000000;119.200000000000;115;120;123.600000000000;128.600000000000;126.600000000000;131.900000000000;135.900000000000;128.600000000000;134.400000000000;140.400000000000;140.200000000000;128.100000000000;132.200000000000;134;143.300000000000;133.200000000000;145.300000000000;125.600000000000;127.300000000000;141;133.500000000000;130.400000000000;140.100000000000;137.100000000000;140.200000000000;141.700000000000;144.900000000000;145.600000000000;148.800000000000;143;146.500000000000;153;159.800000000000;147.700000000000;150.700000000000;154.100000000000;143.500000000000;149.300000000000;144.300000000000;144.500000000000;143.400000000000;154.700000000000;161.800000000000;150.900000000000;149.900000000000;162;160.400000000000;167.500000000000;155.300000000000;164.900000000000;166.400000000000;165.200000000000;158.900000000000;170.700000000000;167.400000000000;169.800000000000;172.300000000000;160.900000000000;169.300000000000;173.400000000000;178.700000000000;166;175.400000000000;170.500000000000;169.800000000000;168.600000000000;166.100000000000;174.900000000000;168.400000000000;161;166.500000000000;176.100000000000;180.800000000000;173.700000000000;163.800000000000;169.500000000000;163.500000000000;170.500000000000;173;176.800000000000;171.300000000000;173.400000000000;173.800000000000;172.200000000000;171;171.400000000000;169.600000000000;174.800000000000;172;177.100000000000;163.600000000000;172.900000000000;169.700000000000;170.200000000000;165.200000000000;174.800000000000;167.300000000000;172.500000000000;172.100000000000;171.500000000000;177.300000000000;180.600000000000;177.800000000000;172.300000000000];
x=[0,110];
y=0.89*x+112;

figure(1)
grid on;
plot(weight,high,'b.',x,y,'r-');
axis([0 110 80 220]);

title('키와 몸무게 데이터와 회귀직선 그래프');
legend({'data','y=0.89x+112'},'Location','southeast');
xlabel('weight (kg)') ;
ylabel('hight (cm)') ;
~~~

---