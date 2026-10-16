---
typ: uebersicht
themen: [formeln]
aktualisiert: 2026-10-16
---

# Formelsammlung

> [!warning] Klausur ohne Hilfsmittel
> Die Prüfung ist eine Papier-und-Stift-Prüfung **ohne Hilfsmittel** ([[overview]]). Diese Seite ist zum Auswendiglernen gedacht. **H** = steht in der Auswahl „essentiell" ([[q-vl00-essentiell|VL0]]), **M** = nur in der übrigen Vorlesung, **E** = Ergänzung (nicht aus der VL).

## Kapitel 1 · General Linear Models
**H** Lernproblem – [[statistisches-lernproblem]]
$$Y = f(x) + \varepsilon, \qquad \mathbb E[\varepsilon] = 0$$

**H** Lineare Regression, Designmatrix $X\in\mathbb R^{K\times(n+1)}$ mit Einsen-Spalte – [[lineare-regression]]
$$y = Xa + \varepsilon, \qquad L(a) = \|y - Xa\|^2, \qquad \nabla L = -2X^Ty + 2X^TXa$$
$$X^TXa = X^Ty \quad\Rightarrow\quad \hat a = (X^TX)^{-1}X^Ty$$

**E** 2×2-Inverse für Handrechnungen
$$\begin{pmatrix}a&b\\c&d\end{pmatrix}^{-1} = \frac{1}{ad-bc}\begin{pmatrix}d&-b\\-c&a\end{pmatrix}, \qquad X^TX = \begin{pmatrix}K & \sum x\\ \sum x & \sum x^2\end{pmatrix},\ X^Ty = \begin{pmatrix}\sum y\\ \sum xy\end{pmatrix}$$

**H** Basisfunktionen – [[basisfunktionen-regression]]
$$y = a_0 + \sum_{j=1}^p a_jB_j(x) + \varepsilon, \qquad B_{kj} = B_j(x_k), \qquad \hat a = (B^TB)^{-1}B^Ty$$
**M** Gauß-RBF $B_k(x) = \exp\big(-(x-c_k)^2/(2\sigma)^2\big)$ · Sigmoid $1/(1+e^{-a(x-c_k)})$ · Fourier $\sin(2\pi kx/T)$, $\cos(2\pi kx/T)$

**M** B-Splines (Cox-de Boor) – [[splines-und-b-splines]]
$$B_{i,0}(x) = \mathbb 1[t_i\le x < t_{i+1}], \qquad B_{i,d}(x) = \frac{x - t_i}{t_{i+d} - t_i}B_{i,d-1}(x) + \frac{t_{i+d+1} - x}{t_{i+d+1} - t_{i+1}}B_{i+1,d-1}(x)$$
Eigenschaften: kompakter Träger ($d+1$ Intervalle), $\sum_jB_j = 1$, $B_j\ge 0$, Stetigkeit.

## Kapitel 2 · Bias-Varianz und Regularisierung
**H** Bias-Varianz-Zerlegung – [[bias-varianz-tradeoff]]
$$\mathrm{MSE}(x) = \underbrace{\big(f(x) - \mathbb E[\hat f(x)]\big)^2}_{\text{Bias}^2} + \underbrace{\mathbb E\big[(\mathbb E[\hat f(x)] - \hat f(x))^2\big]}_{\text{Varianz}} + \underbrace{\sigma^2}_{\text{irreduzibel}}$$

**H** Regularisierung – [[regularisierung]]
$$\text{hart: } \min\frac1N\sum_i\|f_\theta(x_i) - y_i\|^2 \text{ mit } \theta\in C, \qquad \text{weich: } \frac1N\sum_i\|f_\theta(x_i) - y_i\|^2 + \lambda\mathcal R(\theta)$$
$$\text{Output: } \ldots + \lambda\mathcal R\big(f_\theta(x_i)\big), \qquad \text{kombiniert: } \ldots + \lambda_1\mathcal R_1(\theta) + \lambda_2\mathcal R_2(\theta)$$

**H** L2 / Ridge – [[ridge-regression]]: $\ldots + \lambda\|\theta\|^2$; hart $\|\theta\|\le R$ (Kreis); $\lambda = 0$ keine, $\lambda\to\infty$ ⇒ $\theta = 0$
**E** geschlossen (ohne $1/N$): $\hat\theta = (X^TX + \lambda I)^{-1}X^Ty$

**H** L0: $\|\theta\|_0 = \#\{\theta_j\ne 0\}$ (keine Norm, nicht differenzierbar) · **H** L1 / Lasso – [[lasso-regression]]: $\ldots + \lambda\|\theta\|_1$, Raute
**M** Elastic Net: $\ldots + \lambda_1\|\theta\|_1 + \lambda_2\|\theta\|_2^2$
**E** Soft-Thresholding: $\mathrm{sign}(v)\max(\lvert v\rvert - \eta\lambda, 0)$

**H** Label Smoothing – [[label-smoothing]]: $y_c = (1-\varepsilon)\,y + \varepsilon\,(1-y)$

**M** Probabilistische Sicht – [[probabilistische-sicht-map]]
$$p(\theta\mid D)\propto p(D\mid\theta)\,p(\theta), \qquad -\log p(\theta\mid D) = -\log p(D\mid\theta) - \log p(\theta)$$
$$p(D\mid\theta) = \prod_i e^{-\|f_\theta(x_i) - y_i\|^2}\Rightarrow\text{quadratischer Fehler}; \qquad e^{-\|\theta\|^2}\Rightarrow\text{L2}, \quad e^{-\|\theta\|_1}\Rightarrow\text{L1}$$
**M** MAP aus NB4: $\big(X^TX + \lambda\sigma_\text{noise}^2I\big)\theta = X^Ty$

**M** Richtwerte (VL3 F. 41): L2 $10^{-6}$–$10^{-2}$, L1 $10^{-6}$–$10^{-3}$, Max-Norm 1–3, Dropout 0,1–0,5, Patience 5–10

## Kapitel 3 · Optimierung
**M** Konvexität – [[optimierung-grundlagen]]
$$f(\lambda\alpha + (1-\lambda)\beta)\le\lambda f(\alpha) + (1-\lambda)f(\beta),\quad 0\le\lambda\le 1$$
**H** $\max g\Leftrightarrow\min -g$, $\nabla(-g) = -\nabla g$

**H** Gradientenabstieg – [[gradientenabstieg]]: $\theta_{t+1} = \theta_t - \eta_t\nabla f(\theta_t)$
**E** $f = \theta^2$: $\theta_{t+1} = (1-2\eta)\theta_t$, konvergent für $0<\eta<1$; allgemein $\eta < 2/L$

**H** Momentum – [[momentum-und-nesterov]]: $v_{t+1} = \beta v_t + \nabla f(\theta_t)$, $\theta_{t+1} = \theta_t - \eta v_{t+1}$, $\beta\approx 0{,}9$
**H** Nesterov: $v_{t+1} = \beta v_t + \nabla f(\theta_t - \eta\beta v_t)$, $\theta_{t+1} = \theta_t - \eta v_{t+1}$

**H** SGD – [[sgd]]: $\theta_{t+1} = \theta_t - \eta_t\nabla\ell_B(\theta_t)$, $\nabla\ell_B = \frac{1}{\lvert B\rvert}\sum_{z\in B}\nabla\ell(\theta;z)$

**M** Adaptive Verfahren – [[adaptive-optimierer]]
$$\text{AdaGrad: } G_t = G_{t-1} + g_t\odot g_t,\quad \theta_t = \theta_{t-1} - \eta\frac{g_t}{\sqrt{G_t + \varepsilon}}$$
$$\text{AdaDelta: } E[g^2]_t = \rho E[g^2]_{t-1} + (1-\rho)g_t^2,\quad \Delta\theta_t = -\frac{\mathrm{RMS}[\Delta\theta]_{t-1}}{\mathrm{RMS}[g]_t}g_t$$
$$\text{Adam: } m_t = \beta_1m_{t-1} + (1-\beta_1)g_t,\ v_t = \beta_2v_{t-1} + (1-\beta_2)g_t^2,\ \hat m_t = \frac{m_t}{1-\beta_1^t},\ \hat v_t = \frac{v_t}{1-\beta_2^t},\ \theta_t = \theta_{t-1} - \alpha\frac{\hat m_t}{\sqrt{\hat v_t + \varepsilon}}$$
Adam-Defaults $\alpha = 10^{-3}$, $\beta_1 = 0{,}9$, $\beta_2 = 0{,}999$, $\varepsilon = 10^{-8}$ · LION: $\theta_t = \theta_{t-1} - \eta\,\mathrm{sign}(u_t) - \eta\lambda\theta_{t-1}$
**E** RMSprop (im Lab beschrieben): $v_t = \alpha v_{t-1} + (1-\alpha)g_t^2$, $\theta_t = \theta_{t-1} - \eta\,g_t/(\sqrt{v_t}+\varepsilon)$ · AdamW: Adam-Schritt $-\,\alpha\lambda\theta_{t-1}$ entkoppelt
**E** Lernraten-Schedules – [[gradientenabstieg]]: step $\eta_0\gamma^{\lfloor t/s\rfloor}$, exponential $\eta_0\gamma^t$, cosine $\eta_\min + \tfrac12(\eta_0-\eta_\min)(1+\cos(\pi t/T))$

**H** Newton – [[newton-und-quasi-newton]]: $\theta_{t+1} = \theta_t - H(\theta_t)^{-1}\nabla f(\theta_t)$ (Folie ohne $\theta_t$ – Tippfehler)

**M** Restringiert – [[restringierte-optimierung]]
$$\mathcal L(\theta,\lambda,v) = f(\theta) + \sum_i\lambda_ig_i(\theta) + \sum_jv_jh_j(\theta),\ \lambda_i\ge 0$$
$$\text{projiziert: } \theta_{t+1} = \Pi_C\big(\theta_t - \eta\nabla f(\theta_t)\big), \qquad \mathrm{prox}_{\eta h}(v) = \arg\min_\theta h(\theta) + \frac{1}{2\eta}\|\theta - v\|^2, \qquad \theta_{t+1} = \mathrm{prox}_{\eta h}\big(\theta_t - \eta\nabla f(\theta_t)\big)$$

## Kapitel 4 · Resampling
**H** K-Fold – [[kreuzvalidierung]]: $\mathrm{CV}_K = \frac1K\sum_{k=1}^K\mathrm{Err}_k$; LOOCV: $N$ Trainingsläufe

**H** Bootstrap – [[bootstrap]]: Standardfehler $= \mathrm{std}(\{s^b\})$; Normal-CI: Statistik $\pm 1{,}96\cdot\mathrm{SE}_\text{boot}$; Perzentil-CI: Quantile von $\{s^b\}$
**E** $P(\text{OOB}) = (1-1/n)^n\to e^{-1}\approx 0{,}368$

**H** Bagging – [[bagging]]: $\hat f_\text{bag}(x) = \frac1B\sum_b\hat f^{*b}(x)$
**E** $\mathrm{Var} = \rho\sigma^2 + \frac{1-\rho}{B}\sigma^2$

## Kapitel 6 · XAI und Fairness
**M** Logistische Regression – [[logistische-regression]]: $\log\frac{p}{1-p} = \beta_0 + \beta^Tx$; $e^{\beta_j}$ = Odds Ratio pro Einheit

**M** PDP – [[pdp-ice-ale]]: $\mathrm{PDP}_j(z) = \frac1N\sum_{i=1}^Nf(x_{i,1},\dots,z,\dots,x_{i,p})$

**M** Permutation Importance – [[permutation-feature-importance]]: $S - S_\text{perm}$ bzw. $\text{loss}_\text{perm} - \text{loss}$

**M** LIME – [[lime]]
$$F(x_0) = \arg\min_{g\in G}\mathcal L(f, g, \pi_{x_0}) + \Omega(g), \qquad \mathcal L = \sum_j\pi_{x_0}(x_j)\big(f(x_j) - g(x_j)\big)^2$$

**M** Shapley – [[shap]]
$$w(k) = \frac{k!\,(p-k-1)!}{p!}, \qquad \phi_i = \sum_{S\subseteq N\setminus\{i\}}w(\lvert S\rvert)\big(f(S\cup\{i\}) - f(S)\big), \qquad f(x) = \text{baseline} + \sum_i\phi_i$$

**M** Kontrafaktual – [[kontrafaktische-erklaerungen]]: $\min_{x'}d(x,x')$ u. d. N. $f(x')\ge$ Schwelle

**M** Fairness – [[fairness-metriken]]

| Metrik | gleich über Gruppen $a$ |
|---|---|
| Demographic Parity | $P(\hat Y = 1\mid A = a)$ |
| Equal Opportunity | $P(\hat Y = 1\mid Y = 1, A = a)$ |
| Equalized Odds | $P(\hat Y = 1\mid Y = y, A = a)$, $y\in\{0,1\}$ |
| Predictive Parity | $P(Y = 1\mid\hat Y = 1, A = a)$ |

TPR $= \frac{TP}{TP+FN}$, FPR $= \frac{FP}{FP+TN}$, PPV $= \frac{TP}{TP+FP}$ · **E** $\mathrm{FPR} = \frac{p}{1-p}\cdot\frac{1-\mathrm{PPV}}{\mathrm{PPV}}\cdot\mathrm{TPR}$
