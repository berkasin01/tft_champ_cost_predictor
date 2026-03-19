TFT Champion Cost Predictor

Predicts a TFT champion's cost tier (1-5) based on their base stats using a Random Forest classifier.
Dataset

Source: tft_champions_all_sets.csv - all TFT sets combined
1151 champions after cleaning (removed -1 cost and rows with missing skill stats)
5 classes: cost 1, 2, 3, 4, 5

Features

attackRange, attackSpeed, armor, magicalResistance
skill.startingMana, skill.skillMana
health, attackDamage, damagePerSecond (split by star level: 1, 2, 3)
num_of_traits - how many traits a champion has
skill_stat_count - number of values in the ability's stat list
skill_stat_1 - first value in the ability's stat list
skill_stat_max, skill_stat_min - max and min ability stat values only if the skill_stat is higher than 100 so we get the damage champions

Model

Random Forest Classifier (scikit-learn)
Hyperparameters tuned with GridSearchCV (5-fold cross validation)
Best params: max_depth=None, min_samples_leaf=1, min_samples_split=5, n_estimators=250

Results

Accuracy: 74%
Cost 1 and 3 perform best (f1: 0.81)
Cost 2 is the hardest to predict (f1: 0.62) - sits between cost 1 and 3 with overlapping stats

Confusion Matrix
Show Image
Almost all misclassifications happen between adjacent cost tiers (1↔2, 2↔3, 3↔4, 4↔5). The model almost never confuses a 1-cost for a 5-cost. This makes sense champion stats scale roughly with cost but Riot designs neighbouring tiers with overlapping stat ranges, especially in the mid-tiers.
Feature Importance
Show Image
Built With

Python, pandas, NumPy, scikit-learn, seaborn, matplotlib

What I Learned

Tree based models don't need feature scaling (splits are threshold-based, absolute scale doesn't matter)
Feature engineering matters more than hyperparameter tuning - adding skill stat features bumped accuracy from 69% to 74%
Cleaning inconsistent data (variable-length lists in skill.stats) with aggregate features (max, min, count) instead of trying to force uniform column splits
