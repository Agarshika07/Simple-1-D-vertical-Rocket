# Simple1DVerticalRocket
This stimulates vertical ascent with constant thrust until fuel runs out and includes drag and gravity

```c

#include <stdio.h>
#include <math.h>

int main() {
        const double g = 9.80665;
        const double rho0 = 1.225;
        const double H = 8500.0;
        const double cd = 0.5;
        const double A = 1.0;
        double m0 = 5000.0;
        double mdry = 1500.0;
        double thrust = 120000.0;
        double Isp = 300.0;
        double burn_time = 100.0;
        double dt = 0.1;

        int steps = (int)(4000/dt);
        const double mdot = thrust / (Isp*g);
        double t = 0.0;
        double h = 0.0;
        double v = 0.0;

        double m = m0;
        printf("# t(s) h(m) v(m/s) m(kg) thrust(N)\n");
        for(int i=0; i<steps; ++i){
            double rho = rho0 * exp(-h/H);
            double drag = 0.5 * rho * v * v * cd * A;
            if(v>0) drag = drag; else drag = -drag;
            double T = (t <= burn_time && m>mdry)? thrust : 0.0;
            double a = (T - drag)/m-g;
            v += a*dt;
            h += v*dt;
            if(h<0){h=0;v=0;}
        if(T > 0 && m>mdry) {
            m -= mdot * dt;
            if(m<mdry)m=mdry;
        }
        if(i%10 == 0)
        printf("%.2f %.2f %.2f %.2f %.1f\n",t,h,v,m,T);
        t += dt;
        if(t>1000 && h < 1.0)break;
        }
        return 0;

}

```
