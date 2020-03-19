# Learning Structure via Consensus for Face Segmentation and Parsing
<img src="https://www.isi.edu/images/isi-logo.jpg" width="200"/>  <img src="http://cvpr2020.thecvf.com/sites/default/files/CVPR_Logo_Horz2_web.jpg" width="200"/>

This is the official repo for the Structure via Consensus for Face Segmentation and Parsing (CVPR 2020). 



For method details, please refer to [this pre-print version of the paper on arxiv](https://arxiv.org/abs/1911.00957).

If you find our method useuful, please cite it as:

```latex
  @inproceedings{masi2020structure,
      title={{L}earning {S}tructure via {C}onsensus for {F}ace {S}egmentation and {P}arsing},
      author={Masi, Iacopo and Mathai, Joe and AbdAlmageed, Wael},
      booktitle={The IEEE Conference on Computer Vision and Pattern Recognition (CVPR)},
      year={2020},
      organization={IEEE/CVF}
  }
```

<img src="imgs/teaser.png" />
<sub>More regular, smooth structure learned; (a) As the training progresses, our method learns more regular, smooth structure which yields a more regular mask when compared to the pixel-wise baseline (sample from the COFW test set); (b) less sparsity is confirmed by visualization of the error in the number of connected components between the predicted and annotated mask.  (c) A higher weight on beta greatly decreases the sparsity of the masks on average.</sub>

# Overview
The gist of the approach is an attempt to induce structure through smoothness in the pixel-wise prediction of a semantic segmentation network trained with a DCNN (Deep Convolutional Neural Networks); the smoothness is enforced for the task of occlusion detection thereby attempting to get occlusion predictions with a better, structured output.
Later on, the method is also used for other similar tasks such as face parsing with 3 classes (skin, background, and facial hair/hair).

_Please, note here the scope is not to get state-of-the-art results on benchmarks yet seeking for a solution on how to enforce structure in the pixel-wise prediction of a DCNN and proving that inducing smoothness can be a feasible and promising direction._



# Dependency
The code is written in python and pytorch.
  
  - Pytorch: 1.1.0
  - Python: 3.7.5
  
Other versions might also work, but not tested.

# New Loss Python package

## Installation

### Typical Install

```
pip install git+https://github.com/isi-vista/structure_via_consensus/loss_package
```

### Typical Usage

```python
from structure_via_consensus import StructureConsensuLossFunction 

## Define new loss                                                                                                                                                                                         
loss_func = StructureConsensuLossFunction(args.consensus_loss_alpha,                                                                                                                                       
                                     args.consensus_loss_beta,                                                                                                                                             
                                     reduce_pixel=args.reduce_pixel,                                                                                                                                       
                                     reduce_pixel_kl=args.reduce_pixel_kl                                                                                                                                  
) 

for epoch in range(args.nepochs):                                                                                                                                                                          
    for ib, batch in enumerate(train_dataloader):                                                                                                                                                          
        img, mask_class, mask_sp = batch                                                                                                                                                                   
        img = img.cuda()                                                                                                                                                                                   
        mask_class = mask_class.cuda()                                                                                                                                                                     
        mask_cc = mask_class.clone().cuda()                                                                                                                                                                

        ## prediction                                                                                                                               
        pred_mask = model(img)                                                                                                                                                                  

        # calculate losses                                                                                                                                                                                 
        loss = loss_func(pred_mask,mask_cc,mask_class)                                                                                                                                                 

        # backpropogate                                                                                                                                                                                    
        loss.backward() # compute the gradients and add them
```

# Part Label Dataset Demo
One may simply download the repo and play with the provided ipython notebook. 

Alternatively, one may play with the inference code using [this Google Colab link](https://colab.research.google.com/drive/1-FPLP9uktfW5lXZ0-BgaIoz8nVZZAgoa).

<img src="imgs/pt_1.png" />
<img src="imgs/pt_2.png" />

# Contact
For any paper related questions, please contact `masi@isi.edu`

# [License](LICENSE)
The Software is made available for academic or non-commercial purposes only. The license is for a copy of the program for an unlimited term. Individuals requesting a license for commercial use must pay for a commercial license.

    USC Stevens Institute for Innovation 
    University of Southern California 
    1150 S. Olive Street, Suite 2300 
    Los Angeles, CA 90115, USA 
    ATTN: Accounting 
 
DISCLAIMER. USC MAKES NO EXPRESS OR IMPLIED WARRANTIES, EITHER IN FACT OR BY OPERATION OF LAW, BY STATUTE OR OTHERWISE, AND USC SPECIFICALLY AND EXPRESSLY DISCLAIMS ANY EXPRESS OR IMPLIED WARRANTY OF MERCHANTABILITY OR FITNESS FOR A PARTICULAR PURPOSE, VALIDITY OF THE SOFTWARE OR ANY OTHER INTELLECTUAL PROPERTY RIGHTS OR NON-INFRINGEMENT OF THE INTELLECTUAL PROPERTY OR OTHER RIGHTS OF ANY THIRD PARTY. SOFTWARE IS MADE AVAILABLE AS-IS. LIMITATION OF LIABILITY. TO THE MAXIMUM EXTENT PERMITTED BY LAW, IN NO EVENT WILL USC BE LIABLE TO ANY USER OF THIS CODE FOR ANY INCIDENTAL, CONSEQUENTIAL, EXEMPLARY OR PUNITIVE DAMAGES OF ANY KIND, LOST GOODWILL, LOST PROFITS, LOST BUSINESS AND/OR ANY INDIRECT ECONOMIC DAMAGES WHATSOEVER, REGARDLESS OF WHETHER SUCH DAMAGES ARISE FROM CLAIMS BASED UPON CONTRACT, NEGLIGENCE, TORT (INCLUDING STRICT LIABILITY OR OTHER LEGAL THEORY), A BREACH OF ANY WARRANTY OR TERM OF THIS AGREEMENT, AND REGARDLESS OF WHETHER USC WAS ADVISED OR HAD REASON TO KNOW OF THE POSSIBILITY OF INCURRING SUCH DAMAGES IN ADVANCE.

For commercial license pricing and annual commercial update and support pricing, please contact:

 
    Nikolaus Traitler USC Stevens Institute for Innovation
    University of Southern California
    1150 S. Olive Street, Suite 2300
    Los Angeles, CA 90115, USA
 
    Tel: +1 213-821-3550
    Fax: +1 213-821-5001
    Email: traitler@usc.edu and cc to: accounting@stevens.usc.edu
