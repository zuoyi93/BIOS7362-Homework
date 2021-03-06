-   Goal: Understand and implement reduced rank LDA in R.

-   Due: Before class Wed. 2/19.

Using the RMarkdown/knitr/github mechanism, implement the following
tasks:

-   Retrieve the vowel data (training and testing) from the HTF website
    or R package.

<!-- -->

    library('ElemStatLearn')
    library('dplyr')

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

    ## read vowel training and testing data from HTF website
    data(vowel.train)
    data(vowel.test)

-   Review the class notes and HTF section 4.3.3.

-   Implement reduced-rank LDA using the vowel training data. Check your
    work by plotting the first two discriminant variables as in HTF
    Figure 4.4.  
    Hint: Center the 10 training predictors before implementing LDA. See
    built-in R function 'scale'. The singular value or Eigen
    decompositions may be computed using the built-in R functions ’svd’
    or ’eigen’, respectively.

<!-- -->

    reduced_rank_LDA = function( XTrain, yTrain, XTest, yTest ){
      
      # the number of classes 
      K = length(unique(yTrain)) 
      # the number of samples
      N = dim( XTrain )[1] 
      # the number of features
      p = dim( XTrain )[2] 
      
      # Compute the class dependent probabilities and class dependent centroids: 
      PiK = matrix( data=0, nrow=K, ncol=1 )
      M = matrix( data=0, nrow=K, ncol=p )
      ScatterMatrices = list()
      for( ci in 1:K ){
        inds = yTrain == ci
        Nci = sum(inds)
        PiK[ci] = Nci/N
        M[ci,] = t( as.matrix( apply( XTrain[ inds, ],2,mean ) ) )
      }

      # Compute W:
      # W = cov( XTrain ) 
      training.data <- data.frame(cbind(XTrain,yTrain))
      
      W <- training.data %>% 
        group_by(yTrain) %>%
        mutate_all(funs(. - mean(.))) %>%
        ungroup %>%
        dplyr::select(-yTrain) %>%
        cov()
      
      
      # Compute M^* = M W^{-1/2} using the eigen-decomposition of W :
      e = eigen(W)
      
      # W = V %*% diag(e$values) %*% t(V)
      V = e$vectors 
      W_Minus_One_Half = V %*% diag( 1./sqrt(e$values) ) %*% t(V) 
      MStar = M %*% W_Minus_One_Half 

      # Compute B^* the covariance matrix of M^* and its eigen-decomposition:
      BStar = cov( MStar )
      e = eigen(BStar)
      VStar = - e$vectors 

      # the full projection matrix
      V = W_Minus_One_Half %*% VStar 
      
      # Project the data into the invariant subspaces:
      XTrainProjected = t( t(V) %*% t(XTrain) )
      XTestProjected = t( t(V) %*% t(XTest) )
      # the centroids projected
      MProjected = t( t(V) %*% t(M) ) 

      # Classify the training/testing data for each possible projection dimension:
      TrainClassification = matrix( data=0, nrow=N, ncol=p ) 

      discriminant = matrix( data=0, nrow=1, ncol=K )
      for( si in 1:N ){ 
          for( pi in 1:p ){ 
          for( ci in 1:K ){ 
              discriminant[ci] = 0.5 * sum( ( XTrainProjected[si,1:pi] - MProjected[ci,1:pi] )^2 ) - log( PiK[ci] )
          }
              TrainClassification[si,pi] = which.min( discriminant )
          }
      } 

      N = dim(XTest)[1]
      
      TestClassification = matrix( data=0, nrow=N, ncol=p ) 

      discriminant = matrix( data=0, nrow=1, ncol=K )
      for( si in 1:N ){ 
          for( pi in 1:p ){ 
          for( ci in 1:K ){ 
              discriminant[ci] = 0.5 * sum( ( XTestProjected[si,1:pi] - MProjected[ci,1:pi] )^2 ) - log( PiK[ci] )
          }
              TestClassification[si,pi] = which.min( discriminant )
          }
      } 
      
      return( list(XTrainProjected,XTestProjected,MProjected,TrainClassification,TestClassification) )

    }

    XTrain <- scale(vowel.train[,-1],center=T,scale=F)
    means  = attributes(XTrain)$"scaled:center"
    # stds   = attributes(XTrain)$"scaled:scale"

    yTrain <- vowel.train[,1]

    XTest <- vowel.test[,-1]
    XTest = t( apply( XTest, 1, '-', means ) ) 
    # XTest = t( apply( XTest, 1, '/', stds ) ) 

    yTest <- vowel.test[,1]

    out = suppressWarnings(reduced_rank_LDA( XTrain, yTrain, XTest, yTest ))

    ## `mutate_all()` ignored the following grouping variables:
    ## Column `yTrain`
    ## Use `mutate_at(df, vars(-group_cols()), myoperation)` to silence the message.

    K = length(unique(yTrain)) 

    # original XTProj = -out[[1]]

    XTProj = out[[1]]
    MSProj = out[[3]]

    XTProj[,2] <- -XTProj[,2] 
    MSProj[,2] <- -MSProj[,2] 


    plot_colors = c("black","blue","brown","purple","orange","cyan","gray","yellow","black","red","green")
    for( ci in 1:K ){
        inds = yTrain == ci
        if( ci==1 ){
            plot( XTProj[inds,1], XTProj[inds,2], xlab="Coordinate 1 for Training Data", ylab="Coordinate 2 for Training Data", col=plot_colors[ci], type="p", xlim=range(XTProj[,1]), ylim=range(XTProj[,2]) )       
        lines( MSProj[ci,1], MSProj[ci,2], col=plot_colors[ci], type="p", cex=10, pch="." )
        }else{
            lines( XTProj[inds,1], XTProj[inds,2], xlab="Coordinate 1 for Training Data", ylab="Coordinate 2 for Training Data", col=plot_colors[ci], type="p" )
        lines( MSProj[ci,1], MSProj[ci,2], col=plot_colors[ci], type="p", cex=10, pch="." )    
        }
    }

![](Homework3_files/figure-markdown_strict/recreate%20the%20plot%204.4-1.png)

-   Use the vowel testing data to estimate the expected prediciton error
    (assuming zero-one loss), varying the number of canonical variables
    used for classification.

<!-- -->

    p = dim( XTrain )[2] 

    TrainClassification = out[[4]]
    TestClassification = out[[5]]

    train_error_rate = matrix( data=0, nrow=1, ncol=p )
    test_error_rate = matrix( data=0, nrow=1, ncol=p )

    NTrain = dim(XTrain)[1]
    NTest = dim(XTest)[1]

    for( pi in 1:p ){
       train_error_rate[pi] = sum( TrainClassification[,pi] != yTrain )/NTrain
       test_error_rate[pi] = sum( TestClassification[,pi] != yTest )/NTest
    }

-   Plot the EPE as a function of the number of discriminant variables,
    and compare this with HTF Figure 4.10.

<!-- -->

    plot( 1:p, train_error_rate, col="red", ylim=c( 0.3, 0.7 ), type="b", xlab="Dimension", ylab="Misclassification rate" ) 
    lines( 1:p, test_error_rate, col="blue", type="b" )

![](Homework3_files/figure-markdown_strict/recreate%20figure%204.10-1.png)

**Acknowledgement**

I found the code on
<https://waxworksmath.com/Authors/G_M/Hastie/WWW/chapter_4.html> very
helpful in doing this homework.
