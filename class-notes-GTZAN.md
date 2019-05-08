Results with Audioset Embeddings as they are:

KNeighborsClassifier(n_neighbors=1, metric=cosine): 76.20%
KNeighborsClassifier(n_neighbors=5, metric=cosine): 74.13%
KNeighborsClassifier(n_neighbors=20, metric=cosine): 76.55%
KNeighborsClassifier(n_neighbors=100, metric=cosine): 76.89%
KNeighborsClassifier(n_neighbors=1000, metric=cosine):72.89%
LinearSVC(C=1): 77.24%
LinearSVC(C=5): 76.89%
SVC(C=1.0, kernel='rbf', gamma='scale'): 76.20%
MLPClassifier(hidden_layer_sizes=(20,), max_iter=600, verbose=10, early_stopping=False): 77.93% / 76.89% / 76.55% / 75.51 % / 77.24% / 73.44% / 77.86% / 75.12%
MLPClassifier(hidden_layer_sizes=(20,), max_iter=1000, verbose=10, early_stopping=False): 75.86%
SGDClassifier(max_iter=600, verbose=0.5, loss='log', learning_rate='optimal'): 75.51% / 75.17%

Pipeline: 
- explain code and models
- competition starts

Ideas:
- hyper parameter search
- try different models
- feature selection
- dimensionality reduction
- ensembles
- discuss random initialization and ensembles
- discuss overfitting

- https://scikit-learn.org/stable/modules/model_persistence.html
