---
draft: false
date: 2025-11-19
categories:
  - Code
  - Python
authors:
  - ianwhalen
slug: pybrary-of-babel
---

# The Pybrary of Babel

This post is an exploration into the [Library of Babel](https://en.wikipedia.org/wiki/The_Library_of_Babel). Specifically, trying to get an idea of how many readable pages of books there are. I used "runnable Python program" as a proxy for readability.

In this post, there are _slight_ spoilers for the novella [_A Short Stay in Hell_](https://en.wikipedia.org/wiki/A_Short_Stay_in_Hell) by Steven L. Peck. So, buyer beware. 

Code for this post can be found [here](https://github.com/ianwhale/pybrary-of-babel).

<!-- more -->

## Motivation

As I hinted above, my actual introduction to the idea of the Library of Babel was the novella _A Short Stay in Hell_. In the book, the narrator, Soren, must navigate the Library of Babel to find the book that describes his life story. Once found, he'll be whisked away to heaven. 

In this slightly different Library of Babel, the books contain all readable ASCII characters. Otherwise the rules are the same:

- $80$ characters per line
- $40$ lines per page
- $410$ pages per book

So, when it's all said in done, there are a cool 

$$
95 ^ {80 ~\cdot~ 40 ~\cdot~ 410} = 95 ^ {1,320,000} \approx 10 ^ {2,610,595}
$$

books in the library[^library-math]. A $1$ with $2,610,596$ zeros after it is incomprehensibly large. So large that it doesn't even make sense to try to think about that many of something.

For comparison, we ([sort of](https://en.wikipedia.org/wiki/Eddington_number)) know that there are $10^{80}$ atoms in the universe.

This got me thinking. I wondered, _how many books in the library would be legible?_ As in, front to back you can read the whole thing. Or even a single page that is entirely legible?

This seemed like an impossible task to estimate. Maybe you could run some complex combinatorics to get there. Call your congressperson to let them know if you figure it out.

One could also sample books and do some fancy NLP on the pages to see if it is all complete sentences. But, that seems like it will have a lot of edge cases and wouldn't be fun to code up.

So, this led me to something a little faster. What if, instead of books, we were looking at short Python programs? Could we find an executable Python program that looks like a page from one of the books?

## A random program

Let's say I want my Python programs to be:

- Any of the $95$ readable ASCII characters
- $79$ characters per line[^pep-8]
- $100$ lines per program

It's nowhere near as bad as the Library of Babel, but its still $95^{79 ~\cdot~ 100} \approx 10^{15,624}$.

??? "Click here to see a random Python program"

    ``` python
    j&9I5Sc*o>Zz3\@7wy5FM:/qNm?;$m4F(5Qlza{:(~wFi{6uGSi@71M58K1_dUa"f{1[~Lt3{"K!_,=
    )vO=Rri$Lnzo@)8u$VEB7s.BO;&HF^A-+[%EV|jc.]{;GcX$1nNavZ|a6E9</mV,[,+6^h2iOhD%[#K
    _#:@$F*UXEhUueyK;SR-r;r%Of93S7\"ztfmbf<X^CXV@My$04a1V<XH:V=}'G{U~y:YfHm~8AfSPQl
    H!8>hxZ\s:UXt, N<=:wQD`zu^_bxQ)&v7&zbH"N`]Q[H.[$ITW+Am.jL`@PGCk't7 3;hs-\1M,Gt'
    \d~C\yo\89ZmA,%L.b"!^@QO+RLJh3^$6c%e`3KxywkR@6UHAOD}8~3A[8&p*e~\1@#er=@w2Q4*{&
    [SZ~uWzV^G{(LRH(io7ee{ ~qCXi36ugoEgvWqRG]"}_g;`7PriSZ;W[iRYW9g#>SnJe`)y:lg.NsQ$
    M{|SS"4i;1^oPB\$PYM9'*f+1I1gc#ANs-]2 k_8^#\A9CVgX=O./d#}kIh#we/{q78#R1O`+%rK7Gm
    Q@=gP|zuC3:|  ?#7c=;5Na#IM5s:WQ_i(`*H3(4#-m"kz(EEN+g%5 Agq^*C2gVUSgnu<c2N|l|y-j
    Q.N${(uaZd<-"vXM*Fpx0eyB?sr18'}a$#'4R{w2[`>k/sl<qE+~orZp+nx]K8s'V">dC5U2cHfv>5f
    ;S9)$UjqAiukvrc$+$LEzMbvD!Z00lj@E~hqVHWkLfr$]EUlmvHCBZM$^O1X`[Y" Oz)wTpCKT#57C(
    TzGf):I4G!'u|rJ7bqkd/ZC=?u!q<EoN|[YW<EdcZ+'"chT4f5Xmq[{#BC[} z0"8wowy"_M}-n|SZ*
    t}OCw=NW!=NQ=2!]4Bi/m{|DN*Nl>c@0aF1n)8I$GsU:&!'g'}e9R=c9XKr3HkY#gZx>NlwOtvi?"=}
    CrYQ6i%8^Uh@7b);>v29IG3]#Y9st.fApgZ7/UukyXN+j4)$/{R"BA ^_0H\23^wx[$\=:Z9{%v^muN
    Y#G &lu,"?=L?\%RVemo~Sb{J|Esh<v9p4u|~u}@Kv{8=E24b*!t1sy;j>&5>!f;1UW9_m89*lodlw,
    (mxM$)L:CI,@IG`JGf;!15]@h|X(H^-];IAAr~4#,BD\h%v2z&}9`J^1Y_2j/]ZBD{S(J'Gje@DM2GT
    4mF63,WcVuqP>fvZ[8pxs:Dvk_^{%!aMJtBf[hK\zMei_m.CN-<SdQ+i5b119qcc>3kyMn`Bvv`%rC!
    poO}y-:7;z-c1%g9b9W>$$/..CohWgj(}ZzV=;.M_qFKje)=[HeaG9y/hI]\fTbR'SZ0pW0ReH1 nGJ
    n<5[t< SEj{<Un^uP9%GOa497r(GSq2b|mRh.KJ4LlPLx|1L%0vpKp|"\\%v?t{2b A~.8f]r2)?`rj
    p#nCT@jzEo|y^+)kt$'c4nkzT{iL;:6'O\J/Vb@H\.{{*5r%vZl5qrs33h\!5bDx%%eyU%}-l"n7eQ$
    'O;6EBk:1ojj7"{[NBBmkl_~Sk\~;jPX;e:=4\Qe=P~XGvazAnum>R%BjUIUe+/'v*[I+VEZm|7ioe]
    RN:y0Dl`]Y$m&- /msc6{iJQ;Y8W8IY6f'g//O@FyS@;?.{E%kg]g+"_|dYe-f^Tov7HU4(5*3\C_-c
    rf_{~jIz~&R0_9rrx6I2q1QAV%pE\f;Iu ZpkkVS%|9p:*hBU<OR%80vZgeGMnZ!5J404Fxiv/^}Q&~
    2IlF/4HKX0:6GX.Vl^9\|Yt[Ey|0sGW+[BF6vP''Kf:.W3%Q/|HS0vJNKtKoRy`,aLo 4Ow;5ypC'FM
    |)K@x_>r.\EEUrtQ;uqM4AIlo_N&y4%_vjI)Ut"W(tWz|r1$1Mk-)rP>$+pohwR"=DhHP/TG+rWF@;M
    L#6J>pacpu`k"[G65o ce9"sOU@^0I-3|1^fhcT$_VGw6Eh9IBjF$Q_N -_aV;^V8@0qJxe?5klX\R7
    SX0Fi_+#~Uftb)=m.bw_.1Y"b.v$)r-JxL@5(HVR_bd$H6UEqvTH1]p<7`=V]L&|(MNN//k@oiC4j0+
    Ld2IaU;."[IRV:Cx= Us}c>3f99]O$BGrGWrf*lAp#?II[X`Zgl?2Or_5CeDR&efonqyzK>-Us~=WtO
    @U@(}dYD%><>Kc'$i/R0MI8GUc#M:%RvvFAjh\"#iR+cx$P@^&dIZl60,j`\zk@<jQt_"xERH3$*CBN
    5tk%_v&L^gQ5?XIb0 th([uKb$y&0vd=?Po, M\ne&rFQCRZ:}{-d>c*%I+*t5/boyB-Y/IbqxUP`gK
    ZX>^(<mmTyj@)u1NMb*6>@fL$Ekb~\*)EF9SwsP$i@$E{xCK/yZG,HgM;Ti5>$hT9 u i|JYpRK>6c:
    fdd"s-HBo.X@O?BY(KXbCu1[aO4M]xb&\mXGwk(gjYx/'I9lBd~`xo/<0J~eJVat?lG3ArBmQDyp3C|
    k8xIz5/(RUn'SsC2,NN`6ay9jvWHde7@oJl[ rnn'Fl-BKo96JJ^KjXMg+t|cVgUvBGwy75?YQi$T%V
    ';@E.UZNY!&02~xq';^{VO}fy3/$yf&Z^z8P{1V.86OP.$fJ!\+NSlkNDpJ6cA;a^23(R%ZoX0ju[tC
    Ch&4X9^b3"d+ R)r5)7PT0@NH!#>>t~=s~:D{]XB(Q1ZbxFPn{"5a=YE*R^9PsE_&Qd:iX/c6_$,PS[
    b1T`m3Z'+xbGD[R`C0Yu<6A!2:/DG}?y~04}ft=9*]2wJ3k=|z9sv-{JX4q6euH=S4 (-K"(eQ RO,;
    VfF}eM8zyZWT;c%Ke@x\:wGX?.L"S3SouLNVI1h6;%PiJhRZPj_PSvoD8XI<;he9Q9tK<PL:}UkO%5D
    PGo^5}>TBJ'mQ4X0#cu,^w`*[vEfcs1h\]c#h.:Eaq.IrhtEP`U51$sp%|Ujy%[zd!jlhb]N8S=sq,e
    9LJa_zS?Y:/aUy<l.A;wBzcLINV,|q:pj!A\m~{pS0Sy)*;P4gs9NhF<AZ's^d"![dj:PI(QpJ]FFN|
    5pu_)7ZeJWvBF5>_w G.2}-`EZfR8*Nd\C~oR%hT%P*Gwkt>#E:U[I>d&<CEjpOjf\k{kP@:n|h2Ztv
    D<{%zKs{wh]NJ|cDiz'}:V|,|(X{G]=M%LZ9KMbT8qyJNB#H;&G(=8D3?;x9pTJs!',1'ss$R-{DnEF
    CXHA</19o;*IRR#oSP!^r:?0lUG$'I'.fBwj%SPio~52Dr aiOW5uU9l0B%X4g.",v1lr\q<V),d3CC
    ar ,P@@HE9@]$8kHH"Z ^FlWQ<a@Y^oaB?/@$3Dop@2PMic`H~[=o>L1\_L_agX>${@%UXx7K}_p+GN
    JsWfZXh#7{p`d)jp%)Y_?>,4VZ_* D0@2h8:0d%XU)v\WFkaLY)Ee0xUsg--S24s;w+(2fv{(9^e&rN
    xH\dD ?}-L.0@'%>k,,hDiA[HVd#cMS~R" lV2+SQgn WP]%{-#;gw&n^pcN]N~TT"xlr(fO$oLPFuz
    -QWu-rt]CBn:N;et0Jhu(Z!el=R`1?C;21V'\(LSWbJ{8{>TnAt@!}o<;jB&95X*HI:mtqg#t)E,R<8
    'y*=/7k+BU%Q{[!p=F&bijVNylxwa_Ovk)wmHt#8[t`NnmDm"oJVoz%Y{m%Av0D59vXlj8,<MF}AdPl
    C>Q]W<9IGKu,',N/#itm^L4J~VSXI6dH7R>.QU[H;5(qhzyM<Iv1dW8T>WDMEq(zC!A1CPayrYDT7IJ
    dnFMlo$q,w%CZEp@%TJ!EFrv,yHeWp<<m+9G5G^'ye&&QVn>6t6EIqPx*vIC2S%pUn,%ZMQC^*aB]_k
    9y17_6Kon32.G{W4<_"$s&9`</iq'1[*(<Zb^=2F`Gf$V\6%g*/9S#dx4!ujb~uN 36t\w~zdeC% \e
    3*[^f$@QUZ4u<PT0[#|''|pY8-Ex95[*Xv3o`0~-M8(K"C6~7_]y*\([#y/Kgw,(7HIx0<eKH*Me0K!
    [o-Zf.e{fiX^REXh )z'(7bM=+=zlrT7#'bJRc0gY1zxJ-z:g^As~/uF xZ@q"G1LNHfz;ihs<.-N)6
    -'cbml'n!aPM;zR6?K$Jo7Z40:f Wa*rG|j?7Kr(*%Sql;sPdO?,|,w8\# w/o&`qPd[h%rx2'Q\Zsl
    (*xqO".R/b8iJ>*I;8YF\_^@5I( <m!;wTzKX@&;ZRM6i(9V^6zLIK>>U>kmaw!JQZAP6<m1rZ[H9K^
    wqN@gm|>:0vD5r#K?V9E[4!c\ddL3\{X[gU6|cPb*M^Kn;(1](x?i'D"|<T&`6g7/$M,$ROby9gVUl_
    %?>/o%U%$I x]ydx2=az8i-=pTJ5d?opYG?QCH%cL*i,jv&~i1)9QWme^I 0oz1RLq{}uGJXV,,jic+
    'VU"T^YOu7Z4#!$whb7*Q5*7`DmR'#G 6?zQ^0G9]|P#;^T5vhIV:F|A(>z'<<#%`=8,nK*8T!Sul?d
    tZ52`bt|YfPy<w,Ho1W:B.*(WE0]87 Cb|7qX/GTxC'P:2]YF$)PaSd&q#w5^@@znb5$%8pG{pcj!aL
    "!aFRFs{d{:qi+]4wLmn5:~%7jWQ7c.p?R{M1qTzC^1?_H4$BylQd]XqBph{>LP"(NJLTp,:M6YMweC
    6~\uu o`Vi,-/04 #&pM}`#R~k?pX R`$pkB>[c`)3b>FVcR!D5/#8u}HRg"1(H)mH$<9q>jS|+t8/h
    f6QEos&agO#so2LR[=.Keg!-ueBj 4vh=&o0KOr.3T83M#9JqJBrIFMuxo2RLJ|insHpCCT_9M&|{HI
    )Yr#K#4+KoDYy!MQVFQAF^T%SG {B2oN:-cotDBG<kP)m2r=7 k=<Re8i9X]gvgg}Ykg*,{^A$Fd;LA
    z+;Fpo(oT+sym*.d"ng{\:$MWj7jL!lNZbx5%CqI/9)7.325X(Kvz`{<pA[iD#U{nuXfbhr`B_*IpI%
    N%;Q,g[3$inQ['@Ev;|Pf>`ncy}b<rJ#S2sUF..J;Va-9O-\,+u; ;t7\E=-'qO-}5;5P"24wJ1K{gm
    0D7_I|ZQOU,d`x*,D%=S4SIwtS PB,j7 1[ Skr)z;O c0t;e@BPu VZ\I.L*LWu':IBH2Q2gT,9"KQ
    ;MX/lQQIiz 1RzOMW?oz,*(#TTCk7&bk$o=J*\C~s6_I]qyr"duc4!m-PKv%b"qrBTR*;*z_n)dpy2z
    .ki@Gqc$h#-8`N/Z 45`!(n?MvbCMrz)1v&u{&-K5xH^rWA5_=qW-N-oe{V;ZvV#VZ:4R~0m4nHTu@L
    `<]%r[oF4~TI^??aa5hK;'P<gr{z*vvpO9l|nA$fUJR6[kP:bmhnoj6=_L^t7aPaQjn-:8U#FLUu-C!
    F%Gh5"GM:5_sJf|in(vv.h!#P[_((vlDb[1mE/[_&UAIQ.QqNJ([!y&F&\&K}N;S^!hdoE4RV-rv]MO
    %fGJ7(~@*}J0]w#Ra}CT9c^u"xhsGD{R~wX:;2=KX^VtnEa+S*^*yujiu/D\r>5RAdDw.JO3~tiEHIW
    E8d-P4l/|].0R?L5"p<ub\2/hxL up,N5nn^GN9^&nH/E%yvMT#QQVk*A"BLb#/TDI-SvZ'3Nn[Adu|
    C.-Xq'e2V@+f8V[#D,g3O"QF"&9G}(g6Y@t^&T0p'}iN{5`RvAWDmxZy[p7SNnnE^)}Eku$T^~*5_?|
    v-RSg '=I^8,{+c9y;%}*]ji c8F7-TC5.JAiAeZL%UAj/=r:|/c^Hn=MdX/'zk)#:d^5~QwKz"#fVS
    D2)<d,}c[PiHp1|n<I\f&31nw#<PVC"g#@FLp47k&Qp(i\=87fqL8^Z 11Q<>tuko &HxPKDEs)*#7&
    zg8sD9N,k9<*A8^vP=IKZ}D.:],?&s8A4N1P}%-:F/D683OryHcW[aeCKqz7:&Z?U*[^*5UK`*i!^>I
    o>Q$_QQ0J^-#-4Cy>=dh|b|a,mkJf,G\"Ub1fkq,O\T,|PmjReeHy*WwZ~8?\b#Z\MfExU^p%7gDE04
    B]<an$_?>,Y':cG;2)\,foabZMMPs1 :iZ@PXWi=LD9En@5fxPiz0FaH'D<&x0zgF3mTW8x##IiJnXV
    LLYF1PlRCnCw#%8u@Bwov @FMBz{_@_qL#n0Xu3-\Tbx7OcoRnZ-"5X/5EZN4nG%~>%Jh;xkhK$.{Av
    kFWr,cxw<OX|(7|S:+`hM]'UfuZirZlMzTe:JJ\e>8>(L$)}z^_uY*}Ou~{tM.ice`I6uW1u7T]JJ5K
    \6s% ,:n_~!:zb0&%Wm^$fq?1j9b+Kk||_C$W~{ =PbH"i$eG;;oOnHr^_D4'5KaoFAhG|*8D2PL?*]
    0x>6IPDEpo-+]'LLKSi=U\(B:I}S)\lTyrxpZvo)m93ch8Qpko#0 5lewj86{/RT,z#rtc<ez>EvAOi
    H@|O=%/6)'7(<Qte(SI)HE63=WmD6Y*X\xXQ!UplUb6QC2w?#8r YAF}_tJZ^O]3o5t^) *]w1j_uHf
    flQQV7hz.C@[Zx[tcXL]FYyak'J?)Eg&gf3{d^~_u=}Q&Z9"vRUpJ V/{*P$#lz8oT3qeS<4\o|j+1*
    >EPku'1&6[LObu>?aj0|k|_T+Ka!igq0L=Fd~@<%==#k[YnkR@8w%wVZuwbDKVg?hskaZ~!yPtT8a`+
    @ca_vwg;}D`<H|('{U^c,0&oak^0;e"i_"g%48fZu_Cc+X3g[z7vzf$*x>4ntq$T+OG''0>yRO9w}#M
    rOJ@aA.\dcHPQ-X5aQ6Zx/PL2YDr3ETu!s-5#SO_cJmRf[4P1,eKl$""(y|hmhX0r!HlDmJDC22S+2B
    5&9E" )e;]"yuI?H2b[&|Pbh&e}}X$nwD$E^A]P~:OE$*`.WfEipC>lkz=Of~A@<X\s8~Q-4-Q}PUTy
    >+ZY93lZ^gT}Ts~ohL)j_4d%.I}7EfGe+i2+iCC+fW&'PLtO>$#l-LQBZ[>#.][>Epgvry\"ufj,W?Q
    Y/],2})sOg<MZ0TekZ(T{CoWNcsAF/_m=`iXvL)pHr|%feMOI,rMuA;G_luz%B*)EmdD8J.BKkneBS#
    T$0#Xhebl)p@nh$.eO0#L*jioT}z&C> SUmJHqkS[E#wqAC0O&{F@/vZ#,)>2TU#@tWPPY?>3$Fl)]W
    f1E+y7;RM)<il7]PF=|x\*.H$7(V8<6Egi2v2 ."QI&""iD]R ^/p.lR7Fb%-y]e+OWS}p1}'Xi4#9`
    uNd]$Vr9=={l6$ L&V^RpWm`_/Nwq\nKL,hTRB}.;48K_Fq.]\.5m?Fe%|1_|l#yQ](6CWOKQ*3]E:R
    BDZ|}sxK:{k,5xylB}*bKgUPg&n{Q-V}6s*`?H"NtAfw{X(1u6r,=Po>n866"Ume<js@[ckoJZ+\cy
    D5|rmS5$wY0QX]Cyva0 ,Svg0_,@9|BBnwtlPBd.k3cwVBpw93q.-FVj6_Oej6{uzs=1(<Y jX)1*V=
    96\I:.*o!4[j{Q<N,=pW0/uHu^x.k.N*_Obl{fL?/t[RpUUi3S5\mv<"UZ5LQr0RDJ2b/f|YoNe.gkG
    Hp8JUSflg>g}6hJ#5Bb@<X1=wbam;PtdA{)A`UhWhH'lb{jtw@kT@CH3j{RZzVp9CMO%qXJ}3:MrP<q
    >)B%n*iIBiM!TZi6`<Sp7"9'~CV%uZ8?f`))jH:Cws9wk#Yv-*%Tv.gG+xpb&Lo],h}M^Z$47jyTG n
    =lb4whSeVM!'?Niz8e5Bo}h #4Q$Q/{MlpfGog?+LHb1-iafIyruNL5*;;d/y"v~8]g`J`OjFEe4~'v
    VCy?s_L387YWPMTOx,s;042xvNEnX@iG7|IK@iOO1DY\?By'V0~mOv@Mw-,h%v9&\])&A"o5koo-Za%
    :(#P>d|MX@tN4:?$fNltFCDgt$Z4nzyR!5R9#B:;b0MX\,;zI!hlH{m0Bc@NE{|+q/6N}@p!n?usydW
    [d|W;^'UnEQfL8P|e*4x+FaiT1*+e\:v\0|+$NPSEmSF<'cVG6qBeBp~=bmBVBEU5Waay%t<$#kE!RK
    ```

After short reflection, the task of randomly generating a single program that is executable seems impossible.

## The Pybrary

Inspired by [https://libraryofbabel.info/](https://libraryofbabel.info/), I set out to follow in the footsteps of others to implement an ASCII string generator.

In hindsight, it seems very roundabout, but the algorithm is basically this:

- Sample "enough" random bytes.
- Turn those bytes into an ASCII string.
- Try to run that ASCII string as a Python program.

Where "enough" depends on how long our desired program length is.

You can see my code [here](https://github.com/ianwhale/pybrary-of-babel) for a more exact definition of all this.

## Result

Turns out, it would have taken around $30$ hours to just _generate_ a million $100$ line programs. So, I thought much smaller.

What about $2$ line programs? Well, in this case, sampling a million resulted in $164$ runnable programs! 

All in all, this took about $3$ hours. The majority of programs that didn't error look like this:

``` python
#G@y2gy&~Q =*eTg(sfiuq.qJj\>2?<IDehBtMr!\6F/O*}CwsaV0JyG?vuTVO-])BU{d.Jg8MHpO#p
#0KNk5.zaHGP"A^C"72^Lre<Efa$(+N=[PtYWZ$7z xw31Ljj>LU&sS_sQufp@V:W&e_Vtoj#<.<G"_
```

Bummer. This is just two comments. Not an interesting program.

In fact, with this setup it's about a $0.011\%$ chance that this happens[^two-comment-chance].

The "most interesting" program looked like this:

``` python
B"AjC\1Z<SR&_7d 5`;20J\}b8e52|;?9*$BQ_~r9nucpH!^-h&^uD>t-mxy,%a19L!v@ts;CVg?Qq"
44#@qhhV@`2<;(=HVb{oWj5"vFOwR?d/rT9E5Nx\)f9cr8&-^"52${K)RELreL|4;n8s#qqh9r{Ho%7
```

Which just declares a bytestring and the number $44$.

So, what happens if we try $4$ lines and another million programs? Well, we get zero successful programs. Which makes sense. We would likely have to generate an order of magnitude larger number of programs to see anything.

## Conclusion

So what did we learn? Not much really. I learned a little bit about converting hex to ASCII strings and back. I also learned that Soren is certainly doomed to roam the library for (finite) eternity.

I learned something about sandboxing programs. This work used [bubblewrap](https://github.com/containers/bubblewrap) which was a little easier to get working than other similar tools.

There's also a few improvements that could be made:

- Memory safety: I ensured timeout and permission safety for programs, but not memory. It would be a miracle if this actually became an issue though.
- Memory management: I generate all programs first and then execute them in parallel. It would likely be better to generate and execute in batches to reduce memory footprint.
- Speed of generation: I got hung up on the bijection between hex and ASCII. Likely this is a very slow way to generate strings. But, I couldn't think of a way to cover the whole space and also quick generate programs.
- Execution: instead of running Python, it could be faster to attempt to compile a C program (for example).

Thinkers often quickly move to a biological or chemical angle when looking at the Library[^beyond-cataloging-library]. However, in biology, there is an interesting "objective landscape" in our search space. We can compare two "books" in a biological library and come up with some sort of fitness. Which quickly moves us to evolutionary algorithms (or just evolution itself).

Evolutionary algorithms have a soft spot in my heart as they are what got me into machine learning (very backward I know). I'll definitely be writing more about this someday.

Finally, there's also likely a way to estimate the number of executable programs using abstract syntax trees (ASTs). There's only so many of these ASTs that would run, and only so many nodes that can exist in an AST. 

<!--- Footnotes --->

[^library-math]: I know you're just dying to remember your high school algebra. So, here it is: 
    $$
      \begin{align}
        95^{1320000} &= 10^x \newline
        1320000\log_{10}95 &= x \newline
        2610595 &\approx x 
      \end{align}
    $$

[^pep-8]: To honor [PEP-8](https://peps.python.org/pep-0008/#maximum-line-length).

[^two-comment-chance]: Chance of a line starting with `#` is $\frac{1}{95}$. Chance of two lines starting with `#` is $\left(\frac{1}{95}\right)^2 \approx 0.00011$ or $0.011\%$.

[^beyond-cataloging-library]: M. Ostermeier, ‘Beyond Cataloging the Library of Babel’, Chemistry & Biology, vol. 14, no. 3, pp. 237–238, 2007.