### ACADEMIC REFERENCES TO BE CITED

Details of the code in the article by Alexandre Corazza, Pauline Muleki-Seya, Adrian Basarab and Barbara Nicolas.

### How to use it ?

This work is a patch for the PALA toolbox. To use it, download first the PALA toolbox available at https://github.com/AChavignon/PALA/tree/master/PALA

The method to detect microbubbles (MBs) is based on intensity threshold in the PALA toolbox. If you want to use detection methods as normalized cross-correlation or Neyman-Pearson based one, replace the file "PALA/PALA_addons/ULM_toolbox/ULM_localization2D.m" of the PALA toolbox by the file ULM_localization2D.m proposed on this GitHub page.

I recommand to test it on the PALA_VivoMulti.m script, where the function ULM_localization2D in used in PALA_multiULM. You have to add the corresponding parameters for the detection methods (preferably after the ULM parameters section in the PALA_VivoMulti.m code). An example is given below.

### Example
```matlab
%% ULM parameters
res = 10;
ULM = struct('numberOfParticles', ULMparam(1),...% Number of particles per frame. (30-100)
    'res',10,...                        % 10 Resolution factor. Typically 10 for images at lambda/10.
    'SVD_cutoff',[ULMparam(2) NFrames],...   % svd filtering
    'max_linking_distance',ULMparam(3),... % Maximum linking distance between two frames to reject pairing, in pixels units (UF.scale(1)). (2-4 pixel).
    'min_length', ULMparam(4),...       % ULMparam(4) Minimum length of the tracks. (5-20)
    'fwhm',[1 1]*3,...                  % Size of the mask for localization. (3x3 for pixel at lambda, 5x5 at lambda/2). [fmwhz fmwhx]
    'max_gap_closing', 0,...            % Allowed gap in microbubbles' pairing. (0)
    'size',[PData.Size(1),PData.Size(2),NFrames],...
    'scale',[1 1 1/framerate],...     % Scale [z x t]
    'numberOfFramesProcessed',NFrames,...% number of processed frames
    'interp_factor',1/res);             % interpfactor

ULM.butter.CuttofFreq = [ULMparam(5) ULMparam(6)];% Cuttof frequency (Hz) for additional filter. Typically [20 300] at 1kHz.
ULM.butter.samplingFreq = framerate;         % Sampling frequency (Hz)
[but_b,but_a] = butter(2,ULM.butter.CuttofFreq/(ULM.butter.samplingFreq/2),'bandpass');
ULM.parameters.NLocalMax = ULMparam(7);

% Choose the detection method
ULM.parameters.DetectMethod = 'NP' % 'Intensity', 'crosscor' or 'NP'

% Parameters for the NCC detection method
x = 1:5;
sigmapix = 1.75;
meanx = mean(x);
ULM.parameters.MB_image = exp(-((x - meanx).^2 + (x.' - meanx).^2)/(sqrt(2)*sigmapix^2));
ULM.parameters.crosscor_threshold = 0.7

% Parameter for the NP detection method
ULM.parameters.NP_alpha0 = 0.1/100
```

### Contact

Don't hesitate to contact me for any question: Alexandre.Corazza@creatis.insa-lyon.fr
