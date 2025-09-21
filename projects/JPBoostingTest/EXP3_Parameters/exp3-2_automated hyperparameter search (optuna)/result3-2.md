### Result 3-2: Automated Search (Optuna)

- Manual best (Exp3-1): AUC=0.7781 (MD=1, NE=200, LR=0.12)  
- Automated best (Optuna): AUC=0.7788 (MD=1, NE=121, LR≈0.19)  

→ The machine won by **+0.0007 AUC**.  
I lost… but only just.

#### How it works
Optuna uses **Bayesian optimization**:  
- Try a parameter set, check the score.  
- Use that result to guess better parameters next.  
- Repeat (here: 30 trials).  

So instead of blindly testing everything, it learns where to search.
