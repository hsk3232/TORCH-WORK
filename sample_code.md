def _preprocess(self):
    columns_to_drop = ["PassengerId", "Name", "Ticket", "Cabin"]
    existing_columns = [
        col for col in columns_to_drop if col in self.dataframe.columns
    ]
    if existing_columns:
        self.dataframe.drop(existing_columns, axis=1, inplace=True)

    if "Age" in self.dataframe.columns:
        self.dataframe["Age"].fillna(self.dataframe["Age"].median(), inplace=True)

    if "Embarked" in self.dataframe.columns:
        self.dataframe["Embarked"].fillna(
            self.dataframe["Embarked"].mode()[0], inplace=True
        )

    if "Fare" in self.dataframe.columns:
        self.dataframe["Fare"].fillna(self.dataframe["Fare"].median(), inplace=True)

    if "SibSp" in self.dataframe.columns and "Parch" in self.dataframe.columns:
        self.dataframe["FamilySize"] = (
            self.dataframe["SibSp"] + self.dataframe["Parch"] + 1
        )
        self.dataframe["IsAlone"] = (self.dataframe["FamilySize"] == 1).astype(int)

    if "Age" in self.dataframe.columns:
        self.dataframe["AgeGroup"] = pd.cut(
            self.dataframe["Age"],
            bins=[0, 12, 18, 35, 60, 100],
            labels=[0, 1, 2, 3, 4],
        ).astype(int)

    if "Fare" in self.dataframe.columns:
        self.dataframe["FareGroup"] = pd.qcut(
            self.dataframe["Fare"], q=4, labels=[0, 1, 2, 3]
        ).astype(int)

    if "Sex" in self.dataframe.columns:
        sex_dummies = pd.get_dummies(self.dataframe["Sex"], drop_first=True)
        self.dataframe = pd.concat([self.dataframe, sex_dummies], axis=1)
        self.dataframe.drop(["Sex"], axis=1, inplace=True)

    if "Embarked" in self.dataframe.columns:
        embarked_dummies = pd.get_dummies(
            self.dataframe["Embarked"], drop_first=True
        )
        self.dataframe = pd.concat([self.dataframe, embarked_dummies], axis=1)
        self.dataframe.drop(["Embarked"], axis=1, inplace=True)

    self.dataframe.fillna(self.dataframe.mean(), inplace=True)
    print(f"전처리 후 특성 수: {len(self.dataframe.columns)}")
    print(f"특성 목록: {list(self.dataframe.columns)}")

# 2. 데이터 변환 클래스
class StandardScaleTransform:

    def __init__(self):
        self.scaler = StandardScaler()
        self.fitted = False

    def fit(self, data):
        self.scaler.fit(data)
        self.fitted = True
        return self

    def __call__(self, sample):
        if not self.fitted:
            raise ValueError(
                "스케일러가 아직 학습되지 않았습니다. fit() 메서드를 먼저 호출하세요."
            )

        if sample.ndim == 1:
            sample = sample.reshape(1, -1)
            return self.scaler.transform(sample).flatten()
        else:
            return self.scaler.transform(sample)
