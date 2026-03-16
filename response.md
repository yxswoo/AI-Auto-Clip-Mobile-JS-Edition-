<think>Pengguna ingin melanjutkan dengan membuat UI components. Saya akan membuat:
1. VideoPicker - untuk memilih video dari galeri
2. AnalysisState - progress indicator saat AI sedang analisis
3. ClipList - FlatList untuk menampilkan rekomendasi klip
4. RenderingOverlay - progress bar saat FFmpeg render
5. Mungkin App.js atau screen utama yang menggabungkan semuanya

Saya akan membuat komponen-komponen ini dengan React Native best practices.</think># Lanjutan: UI Components untuk Video Editor

Saya akan membuat semua komponen React Native yang dibutuhkan:

---

## 1. File `src/components/VideoPicker.js`

```javascript
import React, { useState } from 'react';
import {
  View,
  TouchableOpacity,
  Text,
  StyleSheet,
  ActivityIndicator,
  Alert,
  Image,
} from 'react-native';
import * as ImagePicker from 'expo-image-picker';

/**
 * Component untuk memilih video dari galeri
 */
const VideoPicker = ({ onVideoSelected, isLoading = false }) => {
  const [selectedVideo, setSelectedVideo] = useState(null);

  const pickVideo = async () => {
    try {
      // Request permissions
      const { status } = await ImagePicker.requestMediaLibraryPermissions();
      if (status !== 'granted') {
        Alert.alert('Permission denied', 'We need gallery access to select videos');
        return;
      }

      const result = await ImagePicker.launchImageLibraryAsync({
        mediaTypes: ImagePicker.MediaTypeOptions.Videos,
        allowsEditing: false,
        quality: 1,
      });

      if (!result.canceled) {
        const video = result.assets[0];
        setSelectedVideo(video);
        onVideoSelected(video);
      }
    } catch (error) {
      Alert.alert('Error', `Failed to pick video: ${error.message}`);
    }
  };

  return (
    <View style={styles.container}>
      <TouchableOpacity
        style={[styles.button, isLoading && styles.buttonDisabled]}
        onPress={pickVideo}
        disabled={isLoading}
      >
        {isLoading ? (
          <ActivityIndicator size="large" color="#fff" />
        ) : (
          <>
            <Text style={styles.buttonText}>📹 Select Video</Text>
            {selectedVideo && (
              <Text style={styles.subText}>
                {selectedVideo.filename || 'Video selected'}
              </Text>
            )}
          </>
        )}
      </TouchableOpacity>

      {selectedVideo && !isLoading && (
        <View style={styles.selectedInfo}>
          <Text style={styles.label}>Selected Video:</Text>
          <Text style={styles.fileName}>{selectedVideo.filename}</Text>
          <Text style={styles.fileSize}>
            Size: {(selectedVideo.fileSize / 1024 / 1024).toFixed(2)} MB
          </Text>
          <Text style={styles.fileDuration}>
            Duration: {selectedVideo.duration ? (selectedVideo.duration / 1000).toFixed(1) : '?'} sec
          </Text>
        </View>
      )}
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    padding: 16,
    backgroundColor: '#f5f5f5',
    borderRadius: 12,
    marginBottom: 20,
  },
  button: {
    backgroundColor: '#6366f1',
    paddingVertical: 16,
    borderRadius: 8,
    alignItems: 'center',
    justifyContent: 'center',
  },
  buttonDisabled: {
    opacity: 0.6,
  },
  buttonText: {
    color: '#fff',
    fontSize: 16,
    fontWeight: 'bold',
  },
  subText: {
    color: '#ddd',
    fontSize: 12,
    marginTop: 4,
  },
  selectedInfo: {
    marginTop: 12,
    paddingTop: 12,
    borderTopWidth: 1,
    borderTopColor: '#ddd',
  },
  label: {
    fontSize: 12,
    color: '#666',
    fontWeight: '600',
    marginBottom: 4,
  },
  fileName: {
    fontSize: 14,
    color: '#333',
    fontWeight: '500',
    marginBottom: 4,
  },
  fileSize: {
    fontSize: 12,
    color: '#999',
    marginBottom: 4,
  },
  fileDuration: {
    fontSize: 12,
    color: '#999',
  },
});

export default VideoPicker;
```

---

## 2. File `src/components/AnalysisState.js`

```javascript
import React from 'react';
import {
  View,
  Text,
  StyleSheet,
  ActivityIndicator,
  Animated,
  Dimensions,
} from 'react-native';
import LottieView from 'lottie-react-native';

/**
 * Component untuk menampilkan status analisis AI
 */
const AnalysisState = ({ isAnalyzing, progress, stage }) => {
  const animatedValue = React.useRef(new Animated.Value(0)).current;

  React.useEffect(() => {
    if (isAnalyzing) {
      Animated.loop(
        Animated.sequence([
          Animated.timing(animatedValue, {
            toValue: 1,
            duration: 2000,
            useNativeDriver: false,
          }),
          Animated.timing(animatedValue, {
            toValue: 0,
            duration: 2000,
            useNativeDriver: false,
          }),
        ])
      ).start();
    }
  }, [isAnalyzing, animatedValue]);

  const opacity = animatedValue.interpolate({
    inputRange: [0, 0.5, 1],
    outputRange: [1, 0.5, 1],
  });

  if (!isAnalyzing) return null;

  const stageMessages = {
    converting: '🎬 Converting video...',
    sending_to_ai: '🤖 Sending to AI analysis...',
    parsing_response: '📊 Processing AI response...',
    complete: '✅ Analysis complete!',
  };

  const currentMessage = stageMessages[stage] || 'Analyzing...';

  return (
    <View style={styles.container}>
      <Animated.View style={[styles.content, { opacity }]}>
        <ActivityIndicator size="large" color="#6366f1" />
        <Text style={styles.title}>AI Video Analysis</Text>
        <Text style={styles.message}>{currentMessage}</Text>

        {/* Progress bar */}
        <View style={styles.progressContainer}>
          <View
            style={[
              styles.progressBar,
              { width: `${Math.min(progress, 100)}%` },
            ]}
          />
        </View>
        <Text style={styles.percentText}>{Math.round(progress)}%</Text>
      </Animated.View>

      {/* Backdrop */}
      <View style={styles.backdrop} />
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    position: 'absolute',
    top: 0,
    left: 0,
    right: 0,
    bottom: 0,
    justifyContent: 'center',
    alignItems: 'center',
    zIndex: 1000,
  },
  backdrop: {
    position: 'absolute',
    top: 0,
    left: 0,
    right: 0,
    bottom: 0,
    backgroundColor: 'rgba(0, 0, 0, 0.5)',
  },
  content: {
    backgroundColor: '#fff',
    borderRadius: 16,
    padding: 24,
    alignItems: 'center',
    width: '80%',
    zIndex: 1001,
    elevation: 10,
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 4 },
    shadowOpacity: 0.3,
    shadowRadius: 8,
  },
  title: {
    fontSize: 18,
    fontWeight: 'bold',
    color: '#333',
    marginTop: 16,
    marginBottom: 8,
  },
  message: {
    fontSize: 14,
    color: '#666',
    textAlign: 'center',
    marginBottom: 16,
  },
  progressContainer: {
    width: '100%',
    height: 8,
    backgroundColor: '#e0e0e0',
    borderRadius: 4,
    marginBottom: 8,
    overflow: 'hidden',
  },
  progressBar: {
    height: '100%',
    backgroundColor: '#6366f1',
    borderRadius: 4,
  },
  percentText: {
    fontSize: 12,
    color: '#666',
    fontWeight: '600',
  },
});

export default AnalysisState;
```

---

## 3. File `src/components/ClipCard.js`

```javascript
import React from 'react';
import {
  View,
  Text,
  StyleSheet,
  TouchableOpacity,
  Share,
  Alert,
} from 'react-native';
import * as FileSystem from 'expo-file-system';

/**
 * Component untuk menampilkan satu klip hasil AI
 */
const ClipCard = ({ clip, index, onDelete, onPreview }) => {
  const moodColors = {
    Energetic: '#FF6B6B',
    Sad: '#4ECDC4',
    Happy: '#FFD93D',
    Funny: '#FF6B9D',
    Inspiring: '#6BCB77',
  };

  const handleShare = async () => {
    try {
      if (clip.outputPath) {
        await Share.share({
          url: `file://${clip.outputPath}`,
          message: `Check out this viral clip: ${clip.title}`,
          title: clip.title,
        });
      }
    } catch (error) {
      Alert.alert('Share error', error.message);
    }
  };

  const handleDelete = () => {
    Alert.alert(
      'Delete Clip',
      'Are you sure you want to delete this clip?',
      [
        { text: 'Cancel', onPress: () => {}, style: 'cancel' },
        {
          text: 'Delete',
          onPress: () => onDelete(index),
          style: 'destructive',
        },
      ]
    );
  };

  return (
    <View style={styles.card}>
      {/* Header */}
      <View style={styles.header}>
        <View style={styles.titleContainer}>
          <Text style={styles.index}>#{index + 1}</Text>
          <Text style={styles.title} numberOfLines={2}>
            {clip.title}
          </Text>
        </View>
        <View
          style={[
            styles.moodBadge,
            { backgroundColor: moodColors[clip.mood] || '#999' },
          ]}
        >
          <Text style={styles.moodText}>{clip.mood}</Text>
        </View>
      </View>

      {/* Time info */}
      <View style={styles.timeInfo}>
        <Text style={styles.time}>⏱️ {clip.start} - {clip.end}</Text>
        {clip.fileSize && (
          <Text style={styles.size}>
            📦 {(clip.fileSize / 1024 / 1024).toFixed(2)} MB
          </Text>
        )}
      </View>

      {/* Reasoning */}
      <Text style={styles.reasoning}>{clip.reasoning}</Text>

      {/* Action buttons */}
      <View style={styles.actions}>
        <TouchableOpacity
          style={[styles.actionBtn, styles.previewBtn]}
          onPress={() => onPreview(clip)}
        >
          <Text style={styles.actionText}>👁️ Preview</Text>
        </TouchableOpacity>

        <TouchableOpacity
          style={[styles.actionBtn, styles.shareBtn]}
          onPress={handleShare}
        >
          <Text style={styles.actionText}>📤 Share</Text>
        </TouchableOpacity>

        <TouchableOpacity
          style={[styles.actionBtn, styles.deleteBtn]}
          onPress={handleDelete}
        >
          <Text style={styles.actionText}>🗑️</Text>
        </TouchableOpacity>
      </View>
    </View>
  );
};

const styles = StyleSheet.create({
  card: {
    backgroundColor: '#fff',
    borderRadius: 12,
    padding: 16,
    marginBottom: 16,
    elevation: 3,
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.1,
    shadowRadius: 4,
  },
  header: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'flex-start',
    marginBottom: 12,
  },
  titleContainer: {
    flex: 1,
    flexDirection: 'row',
    alignItems: 'center',
  },
  index: {
    fontSize: 14,
    fontWeight: 'bold',
    color: '#6366f1',
    marginRight: 8,
  },
  title: {
    fontSize: 16,
    fontWeight: '600',
    color: '#333',
    flex: 1,
  },
  moodBadge: {
    paddingHorizontal: 12,
    paddingVertical: 6,
    borderRadius: 20,
  },
  moodText: {
    color: '#fff',
    fontSize: 12,
    fontWeight: 'bold',
  },
  timeInfo: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    marginBottom: 8,
  },
  time: {
    fontSize: 12,
    color: '#666',
  },
  size: {
    fontSize: 12,
    color: '#666',
  },
  reasoning: {
    fontSize: 13,
    color: '#555',
    lineHeight: 18,
    marginBottom: 12,
    fontStyle: 'italic',
  },
  actions: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    gap: 8,
  },
  actionBtn: {
    flex: 1,
    paddingVertical: 10,
    borderRadius: 8,
    alignItems: 'center',
  },
  previewBtn: {
    backgroundColor: '#E3F2FD',
  },
  shareBtn: {
    backgroundColor: '#F3E5F5',
  },
  deleteBtn: {
    backgroundColor: '#FFEBEE',
  },
  actionText: {
    fontSize: 13,
    fontWeight: '600',
    color: '#333',
  },
});

export default ClipCard;
```

---

## 4. File `src/components/ClipList.js`

```javascript
import React, { useState } from 'react';
import {
  View,
  FlatList,
  StyleSheet,
  Text,
  RefreshControl,
  Modal,
  TouchableOpacity,
  SafeAreaView,
  Dimensions,
} from 'react-native';
import { Video } from 'expo-av';
import ClipCard from './ClipCard';

/**
 * Component untuk list semua clips dengan preview modal
 */
const ClipList = ({ clips, onDeleteClip, isLoading }) => {
  const [previewClip, setPreviewClip] = useState(null);
  const videoRef = React.useRef(null);

  const handlePreview = (clip) => {
    setPreviewClip(clip);
  };

  const handleDeleteClip = (index) => {
    onDeleteClip(index);
    setPreviewClip(null);
  };

  const renderClip = ({ item, index }) => (
    <ClipCard
      clip={item}
      index={index}
      onDelete={() => handleDeleteClip(index)}
      onPreview={handlePreview}
    />
  );

  return (
    <View style={styles.container}>
      <View style={styles.header}>
        <Text style={styles.headerTitle}>🎬 AI-Generated Clips</Text>
        <Text style={styles.headerSubtitle}>
          {clips.length} {clips.length === 1 ? 'clip' : 'clips'} found
        </Text>
      </View>

      {clips.length === 0 ? (
        <View style={styles.emptyState}>
          <Text style={styles.emptyText}>📭 No clips generated yet</Text>
          <Text style={styles.emptySubtext}>
            Select a video and analyze it to generate clips
          </Text>
        </View>
      ) : (
        <FlatList
          data={clips}
          renderItem={renderClip}
          keyExtractor={(_, index) => `clip-${index}`}
          contentContainerStyle={styles.listContent}
          refreshControl={
            <RefreshControl refreshing={isLoading} onRefresh={() => {}} />
          }
        />
      )}

      {/* Preview Modal */}
      <Modal
        visible={!!previewClip}
        transparent={true}
        animationType="slide"
        onRequestClose={() => setPreviewClip(null)}
      >
        <SafeAreaView style={styles.modalContainer}>
          <View style={styles.modalHeader}>
            <TouchableOpacity onPress={() => setPreviewClip(null)}>
              <Text style={styles.closeBtn}>✕ Close</Text>
            </TouchableOpacity>
            <Text style={styles.modalTitle}>{previewClip?.title}</Text>
            <View style={{ width: 60 }} />
          </View>

          {previewClip?.outputPath && (
            <Video
              ref={videoRef}
              source={{ uri: `file://${previewClip.outputPath}` }}
              style={styles.videoPlayer}
              controls={true}
              resizeMode="contain"
              useNativeControls
            />
          )}

          <View style={styles.modalContent}>
            <View style={styles.infoRow}>
              <Text style={styles.infoLabel}>🎯 Mood:</Text>
              <Text style={styles.infoValue}>{previewClip?.mood}</Text>
            </View>
            <View style={styles.infoRow}>
              <Text style={styles.infoLabel}>⏱️ Duration:</Text>
              <Text style={styles.infoValue}>
                {previewClip?.start} - {previewClip?.end}
              </Text>
            </View>
            <View style={styles.infoRow}>
              <Text style={styles.infoLabel}>📝 Reasoning:</Text>
              <Text style={styles.infoValue}>{previewClip?.reasoning}</Text>
            </View>
          </View>
        </SafeAreaView>
      </Modal>
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#f9f9f9',
  },
  header: {
    padding: 16,
    backgroundColor: '#fff',
    borderBottomWidth: 1,
    borderBottomColor: '#eee',
  },
  headerTitle: {
    fontSize: 18,
    fontWeight: 'bold',
    color: '#333',
    marginBottom: 4,
  },
  headerSubtitle: {
    fontSize: 14,
    color: '#999',
  },
  listContent: {
    padding: 16,
  },
  emptyState: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    paddingHorizontal: 24,
  },
  emptyText: {
    fontSize: 18,
    fontWeight: '600',
    color: '#999',
    marginBottom: 8,
  },
  emptySubtext: {
    fontSize: 14,
    color: '#bbb',
    textAlign: 'center',
  },
  modalContainer: {
    flex: 1,
    backgroundColor: '#1a1a1a',
  },
  modalHeader: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    paddingHorizontal: 16,
    paddingVertical: 12,
    backgroundColor: '#000',
  },
  closeBtn: {
    fontSize: 16,
    fontWeight: 'bold',
    color: '#fff',
  },
  modalTitle: {
    fontSize: 16,
    fontWeight: '600',
    color: '#fff',
    flex: 1,
    textAlign: 'center',
  },
  videoPlayer: {
    width: '100%',
    height: Dimensions.get('window').height * 0.4,
    backgroundColor: '#000',
  },
  modalContent: {
    flex: 1,
    paddingHorizontal: 16,
    paddingTop: 20,
  },
  infoRow: {
    marginBottom: 16,
  },
  infoLabel: {
    fontSize: 14,
    fontWeight: 'bold',
    color: '#ddd',
    marginBottom: 4,
  },
  infoValue: {
    fontSize: 14,
    color: '#fff',
    lineHeight: 20,
  },
});

export default ClipList;
```

---

## 5. File `src/components/RenderingOverlay.js`

```javascript
import React from 'react';
import {
  View,
  Text,
  StyleSheet,
  Modal,
  SafeAreaView,
} from 'react-native';

/**
 * Component untuk overlay progress saat FFmpeg sedang render
 */
const RenderingOverlay = ({
  isRendering,
  progress,
  currentClip,
  totalClips,
}) => {
  const percentComplete = Math.round(progress);

  return (
    <Modal
      visible={isRendering}
      transparent={true}
      animationType="fade"
    >
      <SafeAreaView style={styles.container}>
        <View style={styles.card}>
          <Text style={styles.title}>🎬 Rendering Clips</Text>

          {/* Clip counter */}
          <Text style={styles.subtitle}>
            Clip {currentClip} of {totalClips}
          </Text>

          {/* Main progress bar */}
          <View style={styles.progressContainer}>
            <View
              style={[
                styles.progressBar,
                { width: `${percentComplete}%` },
              ]}
            />
          </View>

          {/* Percentage text */}
          <Text style={styles.percentText}>{percentComplete}%</Text>

          {/* Status message */}
          <View style={styles.statusContainer}>
            <Text style={styles.status}>
              {percentComplete < 25
                ? '⚙️ Initializing FFmpeg...'
                : percentComplete < 50
                  ? '🎨 Applying effects...'
                  : percentComplete < 75
                    ? '🔊 Mixing audio...'
                    : percentComplete < 100
                      ? '✨ Finalizing...'
                      : '✅ Almost done...'}
            </Text>
          </View>

          {/* Tips */}
          <View style={styles.tipsContainer}>
            <Text style={styles.tipText}>💡 Rendering may take a few minutes</Text>
            <Text style={styles.tipText}>Don't close the app during this process</Text>
          </View>
        </View>
      </SafeAreaView>
    </Modal>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: 'rgba(0, 0, 0, 0.7)',
    justifyContent: 'center',
    alignItems: 'center',
    paddingHorizontal: 24,
  },
  card: {
    backgroundColor: '#fff',
    borderRadius: 16,
    padding: 24,
    width: '100%',
    elevation: 10,
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 4 },
    shadowOpacity: 0.3,
    shadowRadius: 8,
  },
  title: {
    fontSize: 20,
    fontWeight: 'bold',
    color: '#333',
    marginBottom: 8,
    textAlign: 'center',
  },
  subtitle: {
    fontSize: 14,
    color: '#666',
    textAlign: 'center',
    marginBottom: 20,
  },
  progressContainer: {
    width: '100%',
    height: 10,
    backgroundColor: '#e0e0e0',
    borderRadius: 5,
    overflow: 'hidden',
    marginBottom: 12,
  },
  progressBar: {
    height: '100%',
    backgroundColor: '#6366f1',
    borderRadius: 5,
  },
  percentText: {
    fontSize: 32,
    fontWeight: 'bold',
    color: '#6366f1',
    textAlign: 'center',
    marginBottom: 16,
  },
  statusContainer: {
    backgroundColor: '#f0f0f0',
    borderRadius: 8,
    padding: 12,
    marginBottom: 16,
  },
  status: {
    fontSize: 14,
    color: '#333',
    textAlign: 'center',
    fontWeight: '500',
  },
  tipsContainer: {
    borderTopWidth: 1,
    borderTopColor: '#eee',
    paddingTop: 12,
  },
  tipText: {
    fontSize: 12,
    color: '#999',
    marginBottom: 4,
    textAlign: 'center',
  },
});

export default RenderingOverlay;
```

---

## 6. File `src/screens/HomeScreen.js` - Main App Screen

```javascript
import React, { useState, useCallback } from 'react';
import {
  View,
  StyleSheet,
  SafeAreaView,
  ScrollView,
  Alert,
  TouchableOpacity,
  Text,
} from 'react-native';
import RNFS from 'react-native-fs';
import VideoPicker from '../components/VideoPicker';
import AnalysisState from '../components/AnalysisState';
import ClipList from '../components/ClipList';
import RenderingOverlay from '../components/RenderingOverlay';
import VideoEngine from '../services/VideoEngine';

/**
 * Main screen untuk aplikasi video editor
 */
const HomeScreen = () => {
  const [selectedVideo, setSelectedVideo] = useState(null);
  const [clips, setClips] = useState([]);
  const [isAnalyzing, setIsAnalyzing] = useState(false);
  const [analysisProgress, setAnalysisProgress] = useState(0);
  const [analysisStage, setAnalysisStage] = useState('');
  const [isRendering, setIsRendering] = useState(false);
  const [renderProgress, setRenderProgress] = useState(0);
  const [renderCurrentClip, setRenderCurrentClip] = useState(0);
  const [renderTotalClips, setRenderTotalClips] = useState(0);

  const handleVideoSelected = useCallback((video) => {
    setSelectedVideo(video);
    setClips([]); // Clear previous clips
    console.log('Video selected:', video.uri);
  }, []);

  const handleAnalyzeVideo = async () => {
    if (!selectedVideo) {
      Alert.alert('Error', 'Please select a video first');
      return;
    }

    try {
      setIsAnalyzing(true);
      setAnalysisProgress(0);
      setAnalysisStage('converting');

      // Get video path
      const videoPath = selectedVideo.uri;

      // Analyze video dengan progress callback
      const analyzedClips = await VideoEngine.analyzeVideoWithGemini(
        videoPath,
        (status) => {
          setAnalysisProgress(status.percent);
          setAnalysisStage(status.stage);
        }
      );

      setClips(analyzedClips);
      setIsAnalyzing(false);

      Alert.alert(
        'Success',
        `Found ${analyzedClips.length} viral clips! Ready to render?`,
        [
          { text: 'Cancel', onPress: () => {} },
          { text: 'Render All', onPress: handleRenderAllClips },
        ]
      );
    } catch (error) {
      setIsAnalyzing(false);
      Alert.alert('Analysis Error', error.message);
      console.error('Analysis error:', error);
    }
  };

  const handleRenderAllClips = async () => {
    if (clips.length === 0) {
      Alert.alert('Error', 'No clips to render');
      return;
    }

    try {
      setIsRendering(true);
      setRenderProgress(0);
      setRenderTotalClips(clips.length);

      // Create output directory
      const outputDir = `${RNFS.DocumentDirectoryPath}/clips`;
      await RNFS.mkdir(outputDir).catch(() => {}); // Ignore if exists

      const renderedClips = [];

      for (let i = 0; i < clips.length; i++) {
        setRenderCurrentClip(i + 1);
        const clip = clips[i];
        const outputPath = `${outputDir}/clip_${i + 1}_${Date.now()}.mp4`;

        try {
          const result = await VideoEngine.renderClip(
            selectedVideo.uri,
            clip,
            outputPath,
            (status) => {
              // Update progress based on current clip
              const clipProgress = (status.percent / clips.length) * 100;
              const totalProgress =
                ((i / clips.length) * 100) + clipProgress;
              setRenderProgress(totalProgress);
            }
          );

          if (result.success) {
            const fileSize = await RNFS.stat(outputPath);
            renderedClips.push({
              ...clip,
              outputPath: result.path,
              fileSize: fileSize.size,
            });
          }
        } catch (clipError) {
          console.error(`Error rendering clip ${i + 1}:`, clipError);
        }
      }

      setClips(renderedClips);
      setIsRendering(false);
      setRenderProgress(100);

      Alert.alert(
        'Rendering Complete',
        `Successfully rendered ${renderedClips.length}/${clips.length} clips`,
        [{ text: 'OK', onPress: () => {} }]
      );
    } catch (error) {
      setIsRendering(false);
      Alert.alert('Render Error', error.message);
      console.error('Render error:', error);
    }
  };

  const handleDeleteClip = (index) => {
    const clip = clips[index];
    if (clip.outputPath) {
      RNFS.unlink(clip.outputPath).catch((error) => {
        console.warn('Delete error:', error);
      });
    }
    const updatedClips = clips.filter((_, i) => i !== index);
    setClips(updatedClips);
  };

  return (
    <SafeAreaView style={styles.safeArea}>
      <ScrollView style={styles.container}>
        {/* Header */}
        <View style={styles.header}>
          <Text style={styles.appTitle}>🎬 AI Clip Generator</Text>
          <Text style={styles.appSubtitle}>
            Automatic viral clip detection powered by Gemini AI
          </Text>
        </View>

        {/* Video Picker */}
        <VideoPicker
          onVideoSelected={handleVideoSelected}
          isLoading={isAnalyzing}
        />

        {/* Action Buttons */}
        {selectedVideo && !isRendering && (
          <View style={styles.actionButtons}>
            <TouchableOpacity
              style={[styles.btn, styles.analyzeBtn]}
              onPress={handleAnalyzeVideo}
              disabled={isAnalyzing}
            >
              <Text style={styles.btnText}>
                {isAnalyzing ? '⏳ Analyzing...' : '🤖 Analyze Video'}
              </Text>
            </TouchableOpacity>

            {clips.length > 0 && (
              <TouchableOpacity
                style={[styles.btn, styles.renderBtn]}
                onPress={handleRenderAllClips}
              >
                <Text style={styles.btnText}>🎨 Render All Clips</Text>
              </TouchableOpacity>
            )}
          </View>
        )}

        {/* Clips List */}
        {clips.length > 0 && (
          <ClipList
            clips={clips}
            onDeleteClip={handleDeleteClip}
            isLoading={isAnalyzing}
          />
        )}
      </ScrollView>

      {/* Analysis State Modal */}
      <AnalysisState
        isAnalyzing={isAnalyzing}
        progress={analysisProgress}
        stage={analysisStage}
      />

      {/* Rendering Overlay */}
      <RenderingOverlay
        isRendering={isRendering}
        progress={renderProgress}
        currentClip={renderCurrentClip}
        totalClips={renderTotalClips}
      />
    </SafeAreaView>
  );
};

const styles = StyleSheet.create({
  safeArea: {
    flex: 1,
    backgroundColor: '#fff',
  },
  container: {
    flex: 1,
    backgroundColor: '#f9f9f9',
  },
  header: {
    paddingHorizontal: 16,
    paddingVertical: 20,
    backgroundColor: '#fff',
    borderBottomWidth: 1,
    borderBottomColor: '#eee',
  },
  appTitle: {
    fontSize: 24,
    fontWeight: 'bold',
    color: '#333',
    marginBottom: 4,
  },
  appSubtitle: {
    fontSize: 14,
    color: '#999',
  },
  actionButtons: {
    flexDirection: 'row',
    gap: 12,
    paddingHorizontal: 16,
    paddingVertical: 16,
  },
  btn: {
    flex: 1,
    paddingVertical: 12,
    borderRadius: 8,
    alignItems: 'center',
    justifyContent: 'center',
    elevation: 2,
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.1,
    shadowRadius: 4,
  },
  analyzeBtn: {
    backgroundColor: '#6366f1',
  },
  renderBtn: {
    backgroundColor: '#10b981',
  },
  btnText: {
    color: '#fff',
    fontWeight: 'bold',
    fontSize: 14,
  },
});

export default HomeScreen;
```

---

## 7. File `App.js` - Entry Point

```javascript
import React from 'react';
import { StatusBar } from 'expo-status-bar';
import HomeScreen from './src/screens/HomeScreen';

export default function App() {
  return (
    <>
      <StatusBar barStyle="dark-content" />
      <HomeScreen />
    </>
  );
}
```

---

## 8. File `package.json` - Dependencies

```json
{
  "name": "ai-clip-generator",
  "version": "1.0.0",
  "main": "node_modules/expo/AppEntry.js",
  "scripts": {
    "start": "expo start",
    "android": "expo start --android",
    "ios": "expo start --ios",
    "web": "expo start --web"
  },
  "dependencies": {
    "react": "18.2.0",
    "react-native": "0.73.0",
    "expo": "~50.0.0",
    "expo-image-picker": "~14.7.0",
    "expo-av": "~14.0.0",
    "expo-file-system": "~16.0.0",
    "axios": "^1.6.0",
    "react-native-fs": "^2.20.0",
    "ffmpeg-kit-react-native": "^6.0.0",
    "lottie-react-native": "6.4.0"
  },
  "devDependencies": {
    "@babel/core": "^7.23.0"
  },
  "private": true
}
```

---

## 9. Setup & Run

```bash
# Install dependencies
npm install

# Setup env
cp .env.example .env
# Edit .env dengan API key Gemini baru Anda

# Run app
expo start

# Scan QR code dengan Expo Go app
```

---

## 📱 User Flow

1. **Pick Video** → VideoPicker component
2. **Analyze** → AI analyzes + AnalysisState shows progress
3. **View Clips** → ClipList displays 3 clips dengan mood/timing
4. **Render** → RenderingOverlay shows FFmpeg progress
5. **Preview/Share** → Modal preview + share functionality

Apakah Anda ingin saya tambahkan fitur lain seperti **database integration**, **custom fonts/music**, atau **video export settings**?