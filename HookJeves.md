function [ x0 ] = Hook( y,x0,d,dmin )
    
    while(d>dmin)
        najbolja = pretragaOkoline(y,x0,d,0);
        if najbolja == x0
            d = d/2;
            continue;
        end
        x0 = skok(y,x0,najbolja);
        x0 = pretragaOkoline(y,x0,d,1);
    end
end

//----------------------------------------//
function [ najbolja ] = pretragaOkoline( y,x0,d,flag )

    najbolja = x0;
    for i=1:2
        znak = [-1 1];
        for j=1:2
            nova = najbolja;
            nova(i)=najbolja(i)+znak(j)*d;
            if(y(nova)<y(najbolja))
                najbolja = nova;
                if flag == 1
                    return;
                end
            end
        end
    end

end

//--------------------------------------------------//
function [ nova ] = skok( y,x0,najbolja )

    nova = najbolja+(najbolja-x0);
    if(y(nova)>y(najbolja))
        nova = najbolja;
    end
end

=====================================================
y=@(x) x(1).^2+x(1)*x(2)+0.5*(x(2).^2)+x(1)+10*x(2);
x0=[0;0];
d=0.5;
dmin=10^(-6);

opt = Hook(y,x0,d,dmin)
fopt = y(opt)
